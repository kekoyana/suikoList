<template>
  <div id="app">
    <nav>
      <ul>
        <li>
          <a @click="viewQuiz">クイズ</a>
        </li>
        <li>
          <a @click="viewHeros">強者一覧</a>
        </li>
      </ul>
    </nav>
    <section class="container">
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
nav {
  text-align: center;
}
nav ul {
  margin: 0;
  padding: 0;
}
nav ul li {
  list-style: none;
  display: inline-block;
  width: 18%;
  min-width: 90px;
}
</style>
