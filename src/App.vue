<template>
  <div id="app">
    <Header :correctNums="correctNums" :totalNums="totalNums" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "app",
  components: {
    QuestionBox,
    Header
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctNums: 0,
      totalNums: 0
    };
  },
  mounted: function() {
    this.startGame();
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.correctNums++;
      }
      this.totalNums++;
      if (this.totalNums === 10) {
        confirm("Do you want to try again?")
          ? this.startGame()
          : alert("thx for playing!");
      }
    },
    startGame() {
      fetch(
        "https://opentdb.com/api.php?amount=10&category=9&difficulty=easy",
        {
          method: "get"
        }
      )
        .then(response => {
          return response.json();
        })
        .then(jsonData => {
          this.questions = jsonData.results;
          console.log(jsonData);
        });
      this.correctNums = 0;
      this.totalNums = 0;
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
