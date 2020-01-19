<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>
      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer,index) in answers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
        >{{answer}}</b-list-group-item>
      </b-list-group>
      <b-button
        variant="primary"
        href="#"
        class="mr-2 btn waves-effect waves-light"
        type="submit"
        @click="submitAnswer"
        :disabled="selectedIndex===null || answered ===true"
      >Submit</b-button>
      <b-button
        variant="success"
        href="#"
        @click="next"
        :disabled="totalNums===10 || !answered"
      >Next</b-button>
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
    totalNums: Number
  },
  data() {
    return {
      selectedIndex: null,
      correct_index: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      answers = this.shuffledAnswers;
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
        this.answered = false;
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correct_index = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.correct_index === this.selectedIndex) isCorrect = true;
      this.increment(isCorrect);
      this.answered = true;
    },
    answerClass(index) {
      return !this.answered && this.selectedIndex === index
        ? "selected"
        : this.answered && index === this.correct_index
        ? "correct"
        : this.answered &&
          this.selectedIndex === index &&
          index !== this.correct_index
        ? "wrong"
        : "";
    }
  }
};
</script>

<style lang="scss" scoped>
.list-group {
  margin-bottom: 15px;
  .list-group-item {
    &:hover {
      background-color: grey;
      cursor: pointer;
    }
  }
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.wrong {
  background-color: red;
}
</style>