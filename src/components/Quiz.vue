<template>
  <div class="quiz">
    <button @click="changeHero">強者変更</button>
    <div v-if="hero">
      <dl>
        <div>
          <dt>忠義</dt>
          <dd>{{ this.hero.integrity }}</dd>
          <dt>仁愛</dt>
          <dd>{{ this.hero.mercy }}</dd>
          <dt>勇気</dt>
          <dd>{{ this.hero.courage }}</dd>
        </div>
        <div>
          <dt>腕力</dt>
          <dd>{{ this.hero.strength }}</dd>
          <dt>技量</dt>
          <dd>{{ this.hero.dexterity }}</dd>
          <dt>知力</dt>
          <dd>{{ this.hero.wisdom }}</dd>
        </div>
      </dl>
      <input v-model="inputHero" />
      <button @click="check">決定</button>
      {{ message }}
      <button @click="answer">正解表示</button>
      <div v-if="answerFlg">{{ this.hero.name }} {{ this.hero.kana }}</div>
    </div>
  </div>
</template>

<script>
import heros from "./../heros";

export default {
  name: "Quiz",
  data() {
    return {
      hero: null,
      inputHero: "",
      message: "",
      answerFlg: false,
      randCols: ["strength", "dexterity", "wisdom"],
      columns: {
        integrity: "忠義",
        mercy: "仁愛",
        courage: "勇気",
        body: "体力",
        strength: "腕力",
        dexterity: "技量",
        wisdom: "知力"
      }
    };
  },
  methods: {
    changeHero() {
      this.answerFlg = false;
      this.hero = heros.heros[Math.floor(Math.random() * heros.heros.length)];
    },
    check() {
      if (this.inputHero == this.hero.kana) {
        this.message = "正解";
      } else {
        this.message = "まちがい";
      }
    },
    answer() {
      this.answerFlg = true;
    },
    quiz4() {
      this.randCols = this.columns.keys
        .slice()
        .sort(function() {
          return Math.random() - 0.5;
        })
        .slice(0, 3);
    }
  }
};
</script>

<style scoped>
</style>
