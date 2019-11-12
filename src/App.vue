<template>
  <div id="app">
    <Header />
    <!-- if questions.length is 0, QuestionBox won't be rendered -->
    <QuestionBox 
    v-if="questions.length"
    :currentQuestion="questions[index]"
    :next="next"
    />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0
    }
  },
  methods: {
    next() {
      if (this.index === this.questions.length - 1) {
        this.index = 0;
      } else {
        this.index++;
      }
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
