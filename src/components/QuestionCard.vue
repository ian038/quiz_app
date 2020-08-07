<template>
  <div id="quiz">
    <b-card bg-variant="info" class="quiz-card">
      <p>{{ currentQuestion.question }}</p>
      <hr class="my-4" />
        <b-list-group>
          <b-list-group-item 
          v-for="(answer, index) in answers" 
          :key="index" 
          @click="selectAnswer(index); correctAnswer();" 
          :class="answerClass(index)"
          > 
            {{ answer }}
          </b-list-group-item>
        </b-list-group>
    </b-card>  
    <b-button variant="outline-dark" class="next" @click="next(); nextQuestion()">
      Next Question
    </b-button>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  name: 'QuestionCard',
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data: () => {
    return {
      userAnswer: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false,
      number: 0
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers]
      answers.push(this.currentQuestion.correct_answer)
      return answers
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.userAnswer = null
        this.shuffleAnswers()
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.userAnswer = index
      this.answered = true
    },
    correctAnswer() {
      let isCorrect = false

      if(this.userAnswer === this.correctIndex) {
        isCorrect = true
      }

      this.increment(isCorrect)
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    nextQuestion() {
      this.answered = false
    },
    answerClass(index) {
      let answerClass = ''
      if(this.userAnswer === index && index !== this.correctIndex) {
        answerClass = 'incorrect'
      } else if(this.userAnswer === index && index === this.correctIndex ) {
        answerClass = 'correct'
      } else if(this.answered === true && index === this.correctIndex) {
        answerClass = 'correct'
      }
      return answerClass
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .quiz-card {
    margin-bottom: 5%;
  }

  .list-group-item:hover {
    cursor: pointer;
  }

  .correct {
    background: greenyellow;
  }

  .incorrect {
    background: red;
  }
</style>
