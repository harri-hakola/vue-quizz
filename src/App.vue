<template>
  <div id="app">
    <Header
      v-bind:correctNum="correctNum"
      v-bind:totalNum="totalNum"/>

    <b-container class="justify-content-center">
      <b-row>
        <b-col>
          <Quiz
            v-if="questions.length"
            v-bind:currentQuestion="questions[index]"
            v-bind:next="next"
            v-bind:increment="increment"/>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Quiz from './components/Quiz.vue'
import axios from 'axios'; 

export default {
  name: 'app',
  components: {
    Header,
    Quiz
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctNum: 0,
      totalNum: 0
    }
  },

  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.correctNum++
      }
      this.totalNum++
    }
  },

    //Haetaan Open Trivia Databasesta 20 monivalinta kysymystä. 
    mounted() {
      axios.get('https://opentdb.com/api.php?amount=20&type=multiple')
      .then(response => (this.questions = response.data.results))
    }
    
}
</script>


<style>
#app {
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;  
  text-align: center;
  color: black;
  margin-top: 30px;
}
</style>
