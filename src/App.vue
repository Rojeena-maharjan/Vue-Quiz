<template>
  <div id="app">
    <Header :correctAnswers="correctAnswers" :totalQuestions="totalQuestions"/>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            v-bind:currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :replay="replay"
            :correctAnswers="correctAnswers"
            :end="end"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header";
import QuestionBox from "./components/QuestionBox";

export default {
  name: "app",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctAnswers: 0,
      totalQuestions: 0,
      end: false
    };
  },
  mounted: function() {
    this.fetchQuestions();
  },
  methods: {
    fetchQuestions() {
      fetch("https://opentdb.com/api.php?amount=10&category=9&type=multiple", {
        method: "get"
      })
        .then(response => response.json())
        .then(data => {
          this.questions = data.results;
        });
    },
    next() {
      if (this.index < 9) {
        this.index++;
      } else {
        this.end = true;
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.correctAnswers++;
      }
      this.totalQuestions++;
    },
    replay() {
      this.index = 0;
      this.correctAnswers = 0;
      this.totalQuestions = 0;
      this.questions = [];
      this.end = false;
      this.fetchQuestions();
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
