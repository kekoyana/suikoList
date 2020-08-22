<template>
  <div class="hero_table">
    <v-container class="suiko-table">
      <vue-good-table
        :columns="columns"
        :rows="rows"
        :search-options="{
          enabled: true,
          skipDiacritics: true,
          placeholder: '検索',
        }"
      >
        <div slot="emptystate">
          該当の強者は見つかりません
        </div>
      </vue-good-table>
    </v-container>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import heros from "./../heros";
import { VueGoodTable } from "vue-good-table";

@Component({
  components: {
    VueGoodTable
  }
})
export default class HeroTable extends Vue{
  columns: any = [
    { label: "名前", field: "name" },
    { label: "かな", field: "kana" },
    { label: "体力", field: "body", type: "number" },
    { label: "忠義", field: "integrity", type: "number" },
    { label: "仁愛", field: "mercy", type: "number" },
    { label: "勇気", field: "courage", type: "number" },
    { label: "腕力", field: "strength", type: "number" },
    { label: "技量", field: "dexterity", type: "number" },
    { label: "知力", field: "wisdom", type: "number" },
    {
      label: "操舵",
      field: "rudder",
      sortFn: this.boolSortFn,
      formatFn: this.boolFn
    },
    { label: "所在", field: "place", type: "number" },
    { label: "職業", field: "job" },
    { label: "仲間", field: "pride", type: "number" },
    { label: "口調", field: "tone" },
    { label: "登場年", field: "appearance", type: "number" }
  ];
  rows: any[] = heros.heros;
  boolFn(value: boolean): string {
    return value ? "○" : "";
  }
  boolSortFn(x: number): number {
    return x;
  }
}
</script>

<style scoped>
.suiko-table {
  background-color: white;
  color: black;

  }
</style>
