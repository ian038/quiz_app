<template>
  <div id="app">
   <h1>TRIVIA QUIZ</h1>
    <b-button variant="outline-primary" class="start" @click="startTrivia" v-if="!questions.length">
      Start
    </b-button>
    <p class="score">Score: {{ this.score }} / {{ this.totalQuestions }}</p> 
    <b-container class="container">
      <b-row>
        <b-col sm="6" offset="3" class="col">
          <QuestionCard
            v-if="questions.length"
            :currentQuestion="questions[number]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from 'axios'
import QuestionCard from './components/QuestionCard.vue'

export default {
  name: 'App',
  components: {
    QuestionCard
  },
  data: () => {
    return {
      questions: [],
      number: 0,
      score: 0,
      gameOver: true,
      totalQuestions: 10,
      answered: false
    }
  },
  methods: { 
    async startTrivia() {
      axios.get("https://opentdb.com/api.php?amount=10&type=multiple")
           .then(res => {
            this.questions = res.data.results
           })
    }, 
    increment(isCorrect) {
      if(isCorrect) {
        this.score++ 
      }
    },
    next() {
      this.number++
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Sora:wght@700&display=swap');

#app {
  font-family: 'Sora', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  background-image: url("./assets/Image.jpg");
}
</style>
