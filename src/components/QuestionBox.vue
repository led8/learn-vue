<template>

  <div>

    <b-jumbotron header="FANCY QUIZ" lead="Bootstrap v4 Components for Vue.js 2">

      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>

        <b-list-group-item

          v-for="(answer, index) in answersMethod"
          v-bind:key="index"

          v-on:click="selectAnswer(index)"
          v-bind:class="answerClass(index)"

        >

          {{ answer }}
        </b-list-group-item>

      </b-list-group>

      <b-button class="m-5" variant="primary"

        v-on:click="submitAnswer"

        v-bind:disabled="selectedIndex == null || answered"

      >

        Submit button
      </b-button>

      <b-button variant="success" v-on:click="nextFunction">Navigate to next</b-button>

    </b-jumbotron>

</div>

</template>

<script>
  import _ from 'lodash'

  export default {
    props: {
      currentQuestion: Object,
      nextFunction: Function,
      incrementFunction: Function

    },
    data(){
      return {
        selectedIndex: null,
        correctIndex: null,
        shuffledAnswers: [],
        answered: false
      }
    },
    computed: {
      answersMethod: function(){
        let answers = [...this.currentQuestion.incorrect_answers]
        answers.push(this.currentQuestion.correct_answer)
        return _.shuffle(answers) // It works
      }
    },
    watch: {
      currentQuestion: {
        immediate: true,
        handler(){
          this.selectedIndex = null
          this.answered = false

          let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
          this.shuffledAnswers =  _.shuffle(answers)
          this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        }
      }
    },
    methods: {
      selectAnswer: function(index) {
        this.selectedIndex = index
      },
      submitAnswer: function(){
        let isCorrect = false

        if(this.selectedIndex == this.correctIndex){
          isCorrect = true
        }

        this.answered = true

        this.incrementFunction(isCorrect)
      },
      answerClass: function(index) {
        let answerClass = ''

        if(this.selectedIndex === index){
          answerClass = 'selected'
        }

        if(this.answered && this.correctIndex === index) {
          answerClass= 'correct'
        }

        if(this.answered && this.correctIndex != index && this.selectedIndex === index){
          answerClass = 'incorrect'
        }

        return answerClass
      }
    },
  }

</script>


<style>

  .list-group-item{
    cursor: pointer;
  }
  .list-group-item:hover {
    background: #EEE;
  }

  .selected {
    background: lightblue;
    color: white;
  }
   .selected:hover {
    background: lightblue;
    color: white;
  }

  .correct {
    background: lightgreen;
    color: #fff;
  }
  .correct:hover {
    background: lightgreen;
    color: #fff;
  }

  .incorrect {
    background: red;
    color: #fff;
  }
  .incorrect:hover {
    background: red;
    color: #fff;
  }

</style>
