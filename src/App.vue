<template>
  <v-app id="app">
    <v-card>
      <v-tabs grow>
        <v-tab @click="viewQuiz">クイズ</v-tab>
        <v-tab @click="viewHeros">強者一覧</v-tab>
      </v-tabs>
    </v-card>
    <v-main align="top">
      <component :is="currentView"></component>
    </v-main>
  </v-app>
</template>

<script>
import HeroTable from "./components/HeroTable.vue";
import Quiz from "./components/Quiz.vue";

export default {
  name: "App",
  props: {
    source: String
  },
  components: {
    HeroTable,
    Quiz
  },
  methods: {
    viewHeros() {
      this.currentView = "HeroTable";
    },
    viewQuiz() {
      this.currentView = "Quiz";
    }
  },
  data: () => ({
    currentView: "Quiz",
    drawer: null
  }),
  created() {
    this.$vuetify.theme.dark = true;
  }
};
</script>

<style>
</style>
