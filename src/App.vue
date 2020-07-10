<template>
  <v-app id="inspire">
    <v-tabs centered>
      <v-tab @click="viewQuiz">クイズ</v-tab>
      <v-tab @click="viewHeros">強者一覧</v-tab>
    </v-tabs>
    <v-main>
      <v-container
        class="fill-height"
        fluid
      >
        <component :is="currentView"></component>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import HeroTable from "./components/HeroTable.vue";
import Quiz from "./components/Quiz.vue";

export default {
  name: "App",
  props: {
    source: String,
  },
  components: {
    HeroTable,
    Quiz,
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
    drawer: null,
  }),
  created () {
    this.$vuetify.theme.dark = true
  },
};
</script>

<style>
</style>
