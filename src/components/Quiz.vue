<template>
  <div class="quiz">
    <div v-if="hero">
      <v-container class="control text-center" >
        score: {{ score }}
        <v-row>
          <v-text-field solo v-model="inputHero" autocomplete="on" list="kanas" />
          <datalist id="kanas">
            <option v-for="n in kanas" :key="n">{{n}}</option>
          </datalist>
          <v-btn large color="primary" @click="check">決定</v-btn>
        </v-row>
        <div>
          <div v-if="checked !== null">
            {{ this.hero.name }} {{ this.hero.kana }}
            <div v-if="checked == true" class="right">正解</div>
            <div v-if="checked == false" class="miss">まちがい</div>
            <v-btn @click="questionReset">次へ</v-btn>
          </div>
          <div v-else>
            <v-btn @click="pass" :disabled="isChecked">パス</v-btn>
          </div>
        </div>
      </v-container>
      <v-card>
        <v-row>
          <v-col>
            <v-list>
              <v-list-item>
                <v-list-item-content>忠義</v-list-item-content>
                <v-list-item-content>{{ this.hero.integrity }}</v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>仁愛</v-list-item-content>
                <v-list-item-content>{{ this.hero.mercy }}</v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>勇気</v-list-item-content>
                <v-list-item-content>{{ this.hero.courage }}</v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>舵</v-list-item-content>
                <v-list-item-content>{{ this.hero.rudder ? "○" : "" }}</v-list-item-content>
              </v-list-item>
            </v-list>
          </v-col>
          <v-col>
            <v-list>
              <v-list-item>
                <v-list-item-content>体力</v-list-item-content>
                <v-list-item-content>{{ this.hero.body }}</v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>腕力</v-list-item-content>
                <v-list-item-content>{{ this.hero.strength }}</v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>技量</v-list-item-content>
                <v-list-item-content>{{ this.hero.dexterity }}</v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-content>知力</v-list-item-content>
                <v-list-item-content>{{ this.hero.wisdom }}</v-list-item-content>
              </v-list-item>
            </v-list>
          </v-col>
        </v-row>
        <div v-if="tips">
          <v-row>
            <v-col>
              <v-list>
                <v-list-item>
                  <v-list-item-content>職業</v-list-item-content>
                  <v-list-item-content>{{ this.hero.job }}</v-list-item-content>
                </v-list-item>
              </v-list>
            </v-col>
            <v-col>
              <v-list>
                <v-list-item>
                  <v-list-item-content>所在</v-list-item-content>
                  <v-list-item-content>{{ this.hero.place}}</v-list-item-content>
                </v-list-item>
              </v-list>
            </v-col>
          </v-row>
        </div>
        <div v-else class="text-center">
          <v-row>
            <v-col>
          <v-btn @click="viewTips" :disabled="isChecked">ヒントを表示する</v-btn>
            </v-col>
          </v-row>
        </div>
      </v-card>
      <div class="history">
        <span v-for="history in histories" :key="history">{{ history }}</span>
      </div>
    </div>
    <div v-else>
      <v-col class="text-center">
        <v-btn large @click="questionReset">クイズを始める</v-btn>
      </v-col>
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
      isChecked: false,
      hero: null,
      score: 20
    };
  },
  methods: {
    questionReset() {
      this.inputHero = "";
      this.tips = false;
      this.checked = null;
      this.isChecked = false;
      this.hero = heros.heros[Math.floor(Math.random() * heros.heros.length)];
    },
    check() {
      // 一覧にないものは表示できない
      if (!this.kanas.has(this.inputHero)) return;
      if (this.checked !== null) return;

      this.isChecked = true;
      if (this.inputHero == this.hero.kana) {
        this.checked = true;
        this.score += this.tips ? 1 : 2;
      } else {
        this.checked = false;
        this.score -= 2;
      }
      this.addHistory();
    },
    viewTips() {
      this.tips = true;
    },
    pass() {
      this.score -= 2;
      this.addHistory();
      this.questionReset();
    },
    addHistory() {
      this.histories.push(
        (this.checked ? (this.tips ? "⭕️" : "🏆") : "❌") + this.hero.name
      );
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

.right {
  color: red;
}
.miss {
  color: gray;
}
</style>
