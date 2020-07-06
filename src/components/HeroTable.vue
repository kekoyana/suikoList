<template>
  <div class="hero_table">
    <vue-good-table :columns="columns" :rows="rows" />
  </div>
</template>

<script>
import heros from "./../heros";
import { VueGoodTable } from "vue-good-table";

export default {
  name: "HeroTable",
  components: {
    VueGoodTable
  },
  data() {
    return {
      columns: [
        { label: "名前", field: "name" },
        { label: "体力", field: "body", type: "number" },
        { label: "忠義", field: "integrity", type: "number" },
        { label: "仁愛", field: "mercy", type: "number" },
        { label: "勇気", field: "courage", type: "number" },
        { label: "腕力", field: "strength", type: "number" },
        { label: "技量", field: "dexterity", type: "number" },
        { label: "知力", field: "wisdom", type: "number" },
        { label: "操舵", field: "rudder", type: "boolean" }
      ],
      rows: heros.heros
    };
  }
};
</script>

<style scoped>
</style>
