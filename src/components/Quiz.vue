<template>
  <div class="quiz">
    <div v-if="hero">
      <dl>
        <div>
          <dt>忠義</dt>
          <dd>{{ this.hero.integrity }}</dd>
          <dt>仁愛</dt>
          <dd>{{ this.hero.mercy }}</dd>
          <dt>勇気</dt>
          <dd>{{ this.hero.courage }}</dd>
          <dt>体力</dt>
          <dd>{{ this.hero.body }}</dd>
          <dt>腕力</dt>
          <dd>{{ this.hero.strength }}</dd>
          <dt>技量</dt>
          <dd>{{ this.hero.dexterity }}</dd>
          <dt>知力</dt>
          <dd>{{ this.hero.wisdom }}</dd>
          <dt>舵</dt>
          <dd>{{ this.hero.rudder ? "○" : "" }}</dd>
        </div>
        <div v-if="tips">
          <dt>職業</dt>
          <dd>{{ this.hero.job }}</dd>
          <dt>所在</dt>
          <dd>{{ this.hero.place }}</dd>
        </div>
      </dl>
      <div class="control">
        <div>
          <input size="16" v-model="inputHero" autocomplete="on" list="kanas" />
          <datalist id="kanas">
            <option v-for="n in kanas" :key="n">{{n}}</option>
          </datalist>
          <button @click="check">決定</button>
          <button @click="viewTips">ヒント</button>
          <button @click="pass">パス</button>
        </div>
        <div>
          <div v-if="checked !== null">
            {{ this.hero.name }} {{ this.hero.kana }}
            <div v-if="checked == true" class="right">正解</div>
            <div v-if="checked == false" class="miss">まちがい</div>
            <button @click="questionReset">次へ</button>
          </div>
        </div>
      </div>
      <div class="history">
        <span v-for="history in histories" :key="history">{{ history }}</span>
      </div>
    </div>
    <div v-else>
      <button @click="questionReset">クイズを始める</button>
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
      histories: [],
      inputHero: "",
      tips: false,
      checked: null,
      hero: null
    };
  },
  methods: {
    questionReset() {
      this.inputHero = "";
      this.tips = false;
      this.checked = null;
      this.hero = heros.heros[Math.floor(Math.random() * heros.heros.length)];
    },
    check() {
      // 一覧にないものは表示できない
      if (!this.kanas.has(this.inputHero)) return;
      if (this.checked !== null) return;
      this.checked = this.inputHero == this.hero.kana;
      this.addHistory();
    },
    viewTips() {
      this.tips = true;
    },
    pass() {
      this.addHistory();
      this.questionReset();
    },
    addHistory() {
      this.histories.push(
        (this.checked ? (this.tips ? "○" : "◎") : "×") + this.hero.name
      );
    },
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

.right {
  color: red;
}
.miss {
  color: gray;
}
</style>
