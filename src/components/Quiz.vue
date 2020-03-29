<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>
        <!-- Tuodaan kysymys -->
        <div v-html="currentQuestion.question"></div> 
      </template>
        <!-- Tuodaan sekoitetut vastaukset -->
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers" 
          v-bind:key="index"
          v-on:click.prevent="selectAnswer(index)"
          v-bind:class="answerClass(index)">
        <div v-html="answer"></div>
        </b-list-group-item>
      </b-list-group>
       <!-- Select nappi aktivoituu vain kun vastaus on valittu -->
      <b-button pill variant="primary" v-on:click="submitAnswer" v-bind:disabled="currentIndex === null || answered">Select</b-button>
      <b-button v-on:click="next" pill variant="success">Next</b-button> 
    </b-jumbotron>
  </div>
</template>


<script>
import _ from 'lodash' //tämä mahdollistaa _.shuffle funktion käytön myöhemmin
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data: function() {
    return {
      currentIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false
    }
  },

  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.currentIndex = null
        this.answered = false
        this.shuffleAnswers()
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.currentIndex = index
    },

    //
    submitAnswer() {
      let isCorrect = false
      if (this.currentIndex === this.correctIndex) {
        isCorrect = true
      }
      this.answered = true
      this.increment(isCorrect)
    },

    // vastausten sekoittaminen _.shuffle() funktiolla
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },

    // vastausluokkien määrittelyt
    answerClass(index) {
      let answerClass = ''
      if (!this.answered && this.currentIndex === index) {
        answerClass = 'selected'
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct'
      } else if (this.answered &&
        this.currentIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = 'incorrect'
      }
      return answerClass
    }
  }
}
</script>

<style scoped>

.list-group-item:hover {
  background: lightskyblue;
  cursor: pointer;
}
.btn {
  margin: 15px;
}
.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: lightcoral;
}
</style>