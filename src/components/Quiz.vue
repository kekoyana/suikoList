<template>
  <div class="quiz">
    <div>ひげ</div>
    Quiz!!
    {{ this.heroname }}
    <button @click="changeName">名前変更</button>
  </div>
</template>

<script>
import heros from "./../heros";

export default {
  name: "Quiz",
  data() {
    return {
      heroname: "だみー"
    };
  },
  methods: {
    randHero() {
      return heros.heros[Math.floor(Math.random() * heros.heros.length)];
    },
    changeName() {
      this.heroname = this.randHero().name;
    }
  }
};
</script>

<style scoped>
</style>
