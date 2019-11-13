<template>
  <div id="app">
    <Header :correctNum="correctNum" :answered="answered" />
    <!-- if questions.length is 0, QuestionBox won't be rendered -->
    <QuestionBox 
    v-if="questions.length && index < questions.length"
    :currentQuestion="questions[index]"
    :next="next"
    :increment="increment"
    />
    <CongratMessage 
    v-if="index===questions.length"
    :correctNum="correctNum" :answered="answered" :tryAgain="tryAgain"
    />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import CongratMessage from './components/CongratMessage.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
    CongratMessage
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctNum: 0,
      answered: 0
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
      this.answered++
    },
    tryAgain() {
      this.correctNum = 0
      this.answered = 0
      this.index = 0
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=11&type=multiple', {
      method: 'get'
    })
    .then((response) => {
      return response.json()
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
  margin-top: 15px;
}
</style>
