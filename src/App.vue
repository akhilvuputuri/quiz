<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3"> 
          <!-- sm means that on a small screen or bigger; 6 means take up 6 columns of the total 12 given by Bootstrap which is half the page AND offset of 3 means 3 columns of offset from the left and right
          currentQuestion is the question to be passed onto the QuestionBox component-->
          <!-- There is a short moment when fetching the questions that the question is undefined so put a conditional check that the questions array is populated for it to display, so for brief moment in tiem, questionBox is not rendering without the data available to it-->
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment= "increment"
          /> 
          <!-- Pass data variables and methods to child components using v-bind with custom attributes
          Send next method to QuestionBox component -->
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: { // to use component in the header section, need to add it here
    Header,
    QuestionBox
  },
  data() { // local to the App.vue component
    return {
      questions: [],
      index: 0, // keep track of which question at
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function() { // standard fetch API; pass in questions API and options object with method 'get'; runs whenever mounted happens; chaining of code to get the data wanted
    fetch('https://opentdb.com/api.php?amount=10&category=27', {
      method: 'get'
    })
      .then((response) => { // take the response
        return response.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results
      })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
