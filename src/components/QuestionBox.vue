<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template #lead> {{ currentQuestion?.question }} </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectedAnswer(index)"
          :class="answerClass(index)"
          >{{ answer }}</b-list-group-item
        >
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || isAnswered"
        >Submit</b-button
      >
      <b-button
        variant="success"
        @click="nextQuestion"
        :disabled="numOfTotalAnswers === 10"
        >Next</b-button
      >
      <b-button
        variant="secondary"
        v-if="numOfTotalAnswers === 10"
        @click="emitNewRound"
        >New Round</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  name: "QuestionBox",
  props: {
    currentQuestion: Object,
    nextQuestion: Function,
    increment: Function,
    numOfTotalAnswers: Number,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      isAnswered: false,
    };
  },
  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      return answers;
    },
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.isAnswered = false;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectedAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion?.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.isAnswered = true;
      this.increment(isCorrect);
    },

    answerClass(index) {
      let answerClass = "";

      if (!this.isAnswered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.isAnswered && this.correctIndex === index) {
        answerClass = "correct";
      } else if (
        this.isAnswered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = "incorrect";
      } else {
        answerClass = "";
      }
      return answerClass;
    },
    emitNewRound() {
      this.$emit("newRound");
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item {
  cursor: pointer;
}
.list-group-item:hover {
  background-color: #eee;
}
.btn {
  margin: 0 5px;
}
.selected,
.selected:hover {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.incorrect {
  background-color: red;
}
</style>
