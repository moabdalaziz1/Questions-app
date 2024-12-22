<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :nextQuestion="nextQuestion"
            :increment="increment"
            :numOfTotalAnswers="numOfTotalAnswers"
            @newRound="fetchQuestions"
          />
        </b-col>
      </b-row>
    </b-container>
    <Header
      :numOfCorrectAnswers="numOfCorrectAnswers"
      :numOfTotalAnswers="numOfTotalAnswers"
    />
  </div>
</template>

<script>
import Header from "./components/AppHeader.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numOfCorrectAnswers: 0,
      numOfTotalAnswers: 0,
    };
  },
  methods: {
    fetchQuestions() {
      fetch("https://opentdb.com/api.php?amount=10&category=21&type=multiple", {
        method: "get",
      })
        .then((response) => response.json())
        .then((data) => {
          this.questions = data.results;
        })
        .catch((error) => {
          console.error(error);
        });

      this.numOfTotalAnswers = 0;
      this.numOfCorrectAnswers = 0;
    },
    nextQuestion() {
      if (this.index === 9) return;
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numOfCorrectAnswers++;
      }
      this.numOfTotalAnswers++;
    },
  },
  mounted() {
    this.fetchQuestions();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
