<template>
  <div class="question-box-container">
    <b-jumbotron v-if="!end">
      <template slot="lead">{{currentQuestion.question}}</template>

      <hr class="my-4">

      <b-list-group v-if="answers">
        <b-list-group-item
          v-for="(answer,index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
          :disabled="selectedIndex !== null"
        >{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button @click="next" variant="success" :disabled="selectedIndex === null">Next</b-button>
    </b-jumbotron>
    <b-jumbotron v-else :class="won() ? 'win' : 'loss'">
      <b-button @click="replay" variant="warning">Replay</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
    end: Boolean,
    replay: Function,
    correctAnswers: Number
  },
  computed: {
    answers: function() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false
    };
  },
  methods: {
    won: function() {
      return this.correctAnswers > 5 ? true : false;
    },
    shuffleAnswers: function() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);

      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
      console.log(
        this.currentQuestion,
        this.shuffledAnswers,
        this.correctIndex
      );
    },
    selectAnswer: function(index) {
      this.selectedIndex = index;

      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    answerClass(index) {
      return this.answered && index === this.correctIndex
        ? "correct"
        : this.answered &&
          this.selectedIndex === index &&
          index !== this.correctIndex
        ? "incorrect"
        : "";
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background-color: #eee;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}
.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
  color: white;
}
.win {
  background: green;
}
.loss {
  background: red;
}
</style>


