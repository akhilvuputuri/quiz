<template>
    <div class="question-box-container">
        <b-jumbotron>

        <template slot="lead">
            {{ currentQuestion.question }} 
            <!-- currentQuestion passed from parent component; need to also pass the currentQuestion into Javascript through props -->
        </template>

        <hr class="my-4">

        <b-list-group>
            <b-list-group-item v-for="(answer, index) in answers" :key="answer"
            @click="selectAnswer(index)"
            :class="answerClass(index)"
            >
                {{ answer }}
            </b-list-group-item>
        </b-list-group>

        <!--<p v-for="(answer, index) in answers" :key ="index"
        Can also make index the key; Key just has to be unique and must bind each element in for loop to a unique key-->
        <!-- <p v-for="answer in answers" :key="answer">
            {{ answer }}
        </p> -->


        <!-- Disable submit button until selection is made; use selectedIndex to determine whether selection is made; if selectedIndex is true, disabled = true-->
        <!-- Also need to add a check to ensure that the question is not being answered multiple times-->
        <b-button 
            variant="primary"
            @click="submitAnswer"
            :disabled="selectedIndex === null || answered"
        >Submit</b-button>
        <!-- next function is called on click-->
        <b-button @click="next" variant="success" href="#">
            Next
        </b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    props: { // props is any variable passed from another component to this component
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data() {
        return {
            selectedIndex:null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false
        }
    },
    computed: {
        answers() {
            // make a copy of the same array instead of using the same; combining the correct answers into an array with incorrect answers
            // need to reference currentQuestion as this.currentQuestion as it is a prop
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    // watches for changes to the props and runs when changes; in this case currentQuestion is run when the question changes
    watch: {
        // in this case unlike below, currentQuestin is an object instead of a function
        currentQuestion: {
            // Passing in immediate: true in the option will trigger the callback immediately with the current value of the expression:
            // need to reset selectedIndex to null and answered to false after each change in question
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.answered = 
                this.shuffleAnswers()
            }
        }
        // currentQuestion() {
        //     this.selectedIndex = null // resent wehn question changes
        //     this.shuffleAnswers()
        // }
    },
    methods: {
        // To save the selected answer of the user
        selectAnswer(index){
            this.selectedIndex = index
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer() {
            let isCorrect = false
            
            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.answered = true

            this.increment(isCorrect)
        },
        answerClass(index) {
            let answerClass = '';
            
            // need to use this.answered here; in the template you can just reference the variables but in javascript, need to use this. to reference
            if (!this.answered && this.selectedIndex === index) {
                answerClass = 'selected'
            } else if (this.answered && this.correctIndex === index) {
                answerClass = 'correct'
            } else if (this.answered && (this.selectedIndex === index) && (this.selectedIndex !== this.correctIndex)) {
                answerClass = 'incorrect'
            }

            return answerClass;
        }
    }
}
</script>

<style scoped>
.list-group {
    margin-bottom: 15px;
}

.list-group-item:hover {
    background: #EEE;
    cursor: pointer;
}

.btn {
    margin: 0 5px;
}

.selected {
    background-color: lightblue;
}

.correct {
    background-color: lightgreen;
}

.incorrect {
    background-color: red;
}
</style>
