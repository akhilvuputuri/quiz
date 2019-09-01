<template>
    <div>
        <b-nav>
            <b-nav-item disabled><b>Quiz</b></b-nav-item>
            <b-nav-item disabled>Score: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    // props can also be as an array
    props: [ // props is any variable passed from another component to this component
        'numCorrect',
        'numTotal'
    ],
}
</script>
