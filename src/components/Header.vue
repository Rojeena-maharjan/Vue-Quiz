<template>
  <div>
    <b-nav tabs align="center">
      <b-nav-item active>General Quiz</b-nav-item>
      <b-nav-item disabled>Counter: {{correctAnswers}}/{{totalQuestions}}</b-nav-item>
    </b-nav>
  </div>
</template>
<script>
export default {
  props: {
    totalQuestions: Number,
    correctAnswers: Number
  }
};
</script>

