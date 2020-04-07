<template>
  <div>
    <b-container class="bv-example-row">
      <b-row>
        <b-col offset-sm="3" sm="6">
          <b-jumbotron>
            <template v-slot:lead>
              <p v-html="currentQuestion.question"></p>
            </template>

            <hr class="my-4" />

            <b-list-group class="mb-2">
              <b-list-group-item
                v-for="(option, index) in shuffledAnswers"
                @click="handleOptionSelected(index)"
                :key="index"
                :class="applySelectionClass(index)"
              >{{option}}</b-list-group-item>
            </b-list-group>

            <b-button
              variant="primary"
              class="mr-3"
              @click="handleSubmit"
              :disabled="selectedOptionIndex === null || answered"
            >Submit</b-button>

            <b-button variant="success" @click="handleNext">Next</b-button>
          </b-jumbotron>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  name: "question",
  props: {
    currentQuestion: Object,
    currentOptions: Array,
    handleNext: Function,
    incrementCounter: Function
  },
  data() {
    return {
      shuffledAnswers: [],
      selectedOptionIndex: null,
      correctAnswerIndex: null,
      answered: false
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedOptionIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctAnswerIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    handleOptionSelected(index) {
      this.selectedOptionIndex = index;
    },
    applySelectionClass(index) {
      let answerClass = "";

      if (!this.answered && this.selectedOptionIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.correctAnswerIndex === index) {
        answerClass = "correctAnswer";
      } else if (
        this.selectedOptionIndex === index &&
        this.correctAnswerIndex !== index &&
        this.answered
      ) {
        answerClass = "incorrectAnswer";
      }

      return answerClass;
    },
    handleSubmit() {
      // if the answer selected is correct one, then increment counter for correct answer in app.vue
      const isCorrect =
        this.selectedOptionIndex === this.correctAnswerIndex ? true : false;

      this.answered = true;
      this.incrementCounter(isCorrect);
    }
  }
};
</script>

<style scoped>
.list-group-item:hover {
  background: #eeeeee;
  cursor: pointer;
}

.selected {
  background: lightblue;
}

.correctAnswer {
  background: lightgreen;
}

.incorrectAnswer {
  background: lightcoral;
}
</style>