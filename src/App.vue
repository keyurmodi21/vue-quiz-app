<template>
  <div id="app">
    <Header :correctCount="correctCount" :totalCount="totalCount" />
    <Question
      v-if="questionsData.length >0"
      :currentQuestion="questionsData[currentIndex]"
      :handleNext="handleNext"
      :incrementCounter="incrementCounter"
    />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Question from "./components/Question.vue";

export default {
  name: "app",
  components: {
    Header,
    Question
  },
  data() {
    return {
      questionsData: [],
      currentIndex: 0,
      correctCount: 0,
      totalCount: 0
    };
  },
  async mounted() {
    this.retrieveQuestionsData();
  },
  watch: {
    totalCount: {
      async handler(newvalue) {
        if (newvalue === 10) {
          await this.retrieveQuestionsData();
        }
      }
    }
  },
  methods: {
    async retrieveQuestionsData() {
      const response = await fetch(
        "https://opentdb.com/api.php?amount=10&category=21&type=multiple",
        { method: "GET" }
      );
      if (response.ok) {
        const json = await response.json();

        this.questionsData = json.results;
      } else {
        alert("HTTP-Error: " + response.status);
      }
    },
    handleNext() {
      this.currentIndex++;
    },
    incrementCounter(isAnswerCorrect) {
      this.totalCount++;
      isAnswerCorrect ? this.correctCount++ : "";
    }
  }
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
