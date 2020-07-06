<template>
  <div id="app">
    <section class="container">
      <button @click="viewQuiz">クイズ</button>
      <button @click="viewHeros">強者一覧</button>
      <div class="content">
        <component :is="currentView"></component>
      </div>
    </section>
  </div>
</template>

<script>
import HeroTable from "./components/HeroTable.vue";
import Quiz from "./components/Quiz.vue";

export default {
  name: "App",
  components: {
    HeroTable,
    Quiz
  },
  data() {
    return { currentView: "Quiz" };
  },
  methods: {
    viewHeros() {
      this.currentView = "HeroTable";
    },
    viewQuiz() {
      this.currentView = "Quiz";
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
