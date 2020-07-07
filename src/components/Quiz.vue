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
          <dt>体力</dt>
          <dd>{{ this.hero.body }}</dd>
          <dt>腕力</dt>
          <dd>{{ this.hero.strength }}</dd>
          <dt>技量</dt>
          <dd>{{ this.hero.dexterity }}</dd>
          <dt>知力</dt>
          <dd>{{ this.hero.wisdom }}</dd>
        </div>
        <div v-if="tips">
          <dt>舵</dt>
          <dd>{{ this.hero.rudder ? "○" : "" }}</dd>
          <dt>職業</dt>
          <dd>{{ this.hero.job }}</dd>
          <dt>所在</dt>
          <dd>{{ this.hero.place }}</dd>
        </div>
      </dl>
      <div>
        <input v-model="inputHero" autocomplete="on" list="kanas" />
        <datalist id="kanas">
          <option v-for="n in kanas" :key="n">{{n}}</option>
        </datalist>
        <button @click="check">決定</button>
        {{ message }}
      </div>
      <div>
        <button @click="viewTips">ヒント</button>
        <button @click="answer">正解表示</button>
        <div v-if="answerFlg">{{ this.hero.name }} {{ this.hero.kana }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import heros from "./../heros";

export default {
  name: "Quiz",
  data() {
    return {
      kanas: new Set(heros.heros.map(h => h.kana).sort()),
      hero: null,
      inputHero: "",
      message: "",
      answerFlg: false,
      tips: false,
    };
  },
  methods: {
    changeHero() {
      this.answerFlg = false;
      this.message = "";
      this.inputHero = "";
      this.tips = false;
      this.hero = heros.heros[Math.floor(Math.random() * heros.heros.length)];
    },
    check() {
      if (this.inputHero == this.hero.kana) {
        this.message = "正解";
        this.answerFlg = true;
      } else {
        this.message = "まちがい";
      }
    },
    answer() {
      this.answerFlg = true;
    },
    viewTips() {
      this.tips = true;
    }
  }
};
</script>

<style scoped>
dt {
  float: left;
  clear: left;
  margin-right: 0.5em;
  width: 120px;
}

dd {
  float: left;
  margin-left: 1em;
}

input {
  /* 入力欄にフォーカスが当たっても拡大しない */
  font-size: 17px;
}
</style>
