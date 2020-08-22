<template>
  <div class="quiz">
    <div v-if="hero">
      <v-container class="control text-center">
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
        <div>水滸伝天命の誓いの強者の能力から</div>
        <div>どの強者なのかひらがなで答えてください。</div>
        <v-btn large @click="questionReset">クイズを始める</v-btn>
      </v-col>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import Heros from "./../heros";

const WIN_SCORE = 2;
const WIN_SCORE_WITH_HIT = 1;
const LOSE_SCORE = -2;
const PASS_SCORE = -2;

interface Hero {
  id: number;
  name: string;
  kana: string;
  birth: number;
  group: string;
  place: number;
  body: number;
  integrity: number;
  mercy: number;
  courage: number;
  strength: number;
  dexterity: number;
  wisdom: number;
  pride: number;
  tone: string;
  gender: string;
  job: string;
  rudder: boolean;
  appearance: number;
}

//@Component({
//  components: {
//    Heros
//  }
//})
@Component({})
export default class Quiz extends Vue{
  inputHero: string = "";
  tips: boolean = false;
  checked: boolean | null = null;
  isChecked: boolean = false;
  score: number = 0;
  hero: Hero | null = null;
  kanas: Set<string> = new Set(
    Heros.heros
      .sort(function (a, b): number {
        if (a.kana < b.kana) return -1;
        if (b.kana <= a.kana) return 1;
        return 0;
      })
      .map((h) => this.heroName(h))
  );
  histories: any = [];
  questionReset() {
    this.inputHero = "";
    this.tips = false;
    this.checked = null;
    this.isChecked = false;
    this.hero = Heros.heros[Math.floor(Math.random() * Heros.heros.length)];
  }
  check() {
    // 一覧にないものは表示できない
    if (!this.kanas.has(this.inputHero)) return;
    if (this.checked !== null) return;
    if (this.hero == null) return;

    this.isChecked = true;
    if (this.inputHero == this.heroName(this.hero)) {
      this.checked = true;
      this.score += this.tips ? WIN_SCORE_WITH_HIT : WIN_SCORE;
    } else {
      this.checked = false;
      this.score += LOSE_SCORE;
    }
    this.addHistory();
  }
  viewTips() {
    this.tips = true;
  }
  pass() {
    this.score += PASS_SCORE;
    this.addHistory();
    this.questionReset();
  }
  addHistory() {
    if (this.hero == null) return;
    this.histories.push(
      (this.checked ? (this.tips ? "⭕️" : "🏆") : "❌") + this.hero.name
    );
  }
  heroName(hero: Hero): string {
    return `${hero.name}(${hero.kana})`;
  }
}
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
