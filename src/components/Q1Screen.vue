<template>
  <div class="start-screen" v-if="next">
    <div class="game__wrapper">
      <div class="img__wrapper">
        <!-- <img src="~@/assets/icon/dialog-background.svg" alt class="img-bg" /> -->
        <img src="~@/assets/icon/part-upper.svg" alt class="img-bg top" />
        <img src="~@/assets/icon/part-lower.svg" alt class="img-bg bottom" />
        <div class="buyer_question" :class="{active: check}">
          <p>{{ buyerQuestion }}</p>
        </div>

        <div
          class="seller_answer"
          :class="[{active: !check}, checkedAnswer == correctAnswer ? 'good' : 'bad']"
        >
          <p>{{ checkedAnswer }}</p>
        </div>

        <!-- <template v-if="check"> -->
        <div class="stan" :class="{ active: check}">
          <!-- <img src="~@/assets/icon/seller-inactive.svg" alt="seller" class="img-seller inactive" />
          <img src="~@/assets/icon/man-active.svg" alt="customer" class="img-buyer active" />-->
          <img :src="buyer.firstState" alt="customer" class="img-buyer active" />
          <img :src="seller.secondState" alt="seller" class="img-seller inactive" />
          <img src="~@/assets/icon/customer-emotion.svg" alt="emotion" class="buyer-emotion" />
        </div>
        <!-- </template> -->
        <!-- <template v-else> -->
        <div class="stan" :class="{ active: !check}">
          <!-- <img src="~@/assets/icon/seller-active.svg" alt="seller" class="img-seller active" />
          <img src="~@/assets/icon/man-inactive.svg" alt="customer" class="img-buyer inactive" />-->

          <img :src="buyer.secondState" alt="customer" class="img-buyer inactive" />
          <img :src="seller.firstState" alt="seller" class="img-seller active" />
          <img
            v-show="correctAnswer == checkedAnswer"
            src="~@/assets/icon/seller-emotion-good.svg"
            alt="emotion"
            class="seller-emotion"
          />
          <img
            v-show="correctAnswer !== checkedAnswer"
            src="~@/assets/icon/seller-emotion-bad.svg"
            alt="emotion"
            class="seller-emotion"
          />
        </div>
        <!-- </template> -->
      </div>

      <div class="question__wrapper">
        <p
          class="question"
          :class="[smallQuestion, {hide: !check}]"
        >{{ literals.recommedit_questionheader }}</p>
        <template v-if="check">
          <button @click="detectAnswer" class="question-btn main-btn">{{ shuffle[0] }}</button>
          <button @click="detectAnswer" class="question-btn main-btn">{{ shuffle[1] }}</button>
        </template>
      </div>

      <div class="result__wrapper" v-if="!check">
        <div class="check-wrap">
          <img
            v-show="correctAnswer == checkedAnswer"
            src="~@/assets/icon/check-good.svg"
            alt="emotion"
            class="check-type"
          />
          <img
            v-show="correctAnswer !== checkedAnswer"
            src="~@/assets/icon/check-bad.svg"
            alt="emotion"
            class="check-type"
          />
        </div>

        <template v-if="correctAnswer == checkedAnswer">
          <p class="result-text">{{literals.recommendit_correct}}</p>
          <button class="main-btn" @click="theEnd">{{ literals.recommendit_next }}</button>
        </template>
        <template v-else>
          <p class="result-text">{{literals.recommendit_wrong}}</p>
          <div class="btn-line">
            <button class="main-btn" @click="restartGame">{{literals.recommendit_retrybutton}}</button>
            <button class="main-btn next-question" @click="theEnd">{{ literals.recommendit_next }}</button>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  name: "Q1Screen",
  props: {
    next: {
      type: Boolean,
      default: false
    },
    literals: {
      type: Object
    },
    seller: {
      type: Object
    }
  },

  data() {
    return {
      info: null,
      currentQuestion: null,
      question: "",
      answer: ["", ""],
      check: true,
      buyer: null,
      buyerQuestion: "",
      correctAnswer: "",
      checkedAnswer: "",
      questionId: 0,
      prevQuestion: 0,
      firstQuestion: 0,
      hasNext: false,
      fails: 0
    };
  },

  mounted() {
    //get gameID from query parametr
    const url = new URL(window.location.href);
    this.literals.gameId = url.searchParams.get("game");

    axios
      .get(
        `https://iframe-mta-stage-games.brights.io/RecommendIt/Next?game=${
          this.literals.gameId ? this.literals.gameId : 2
        }&QUERY=LDDFnDrHsxDhH5Cfz6Iu4peu4jBptK%2BNxjAh1uThMtsNxuaVG90OUN8GO0t5RADymTWH5VFelL8Ig1R0IGWbIuqGtf4WIb%2Blv2AqMtimNMHypp8DWCrFa%2B3YHWs34%2Fshdb9SaDmPbf%2FlYhqppPbHQkdzA4KP29Xj0k3WXyjYtYRqBejC3hw9NPLcKM2yjTwBmexznskVUM8TD9Zk5W9LdLFGwbu3XFR0OMmxN2Ejvmbtp82yKgReMrW8Y3pr4D6NBcv04m06GoSKHsIs6Ckeg0wS%2BcehiYtlRddi3GsHHJwF3BcD%2BjIk%2Bxo3%2Bz8rmEm5xW31abDQfLElP4OzNlGAedKQZ7oLSL4mPe2ZO6LwpuCC4Zq33zcDtuNKkbhUQr0z5M0UMVJzRTKe%2FQwcbH4Yrjt2dbRGWirNGHcb4mqfL1a45BCjlmDnmWGScUfq3h2wP%2FvS9NNiZFazy%2B0CfVcbRZvPcsjBmGCF2oYASDzs6WXoWrifTfsuDf%2FeGvaoTJUQyFCfogaNU8HXks75nxS5ELOyIeQBvJy1RyQmaKtUZQxyTcQg%2BI9VOnIx%2FwN%2F%2BpfbL9GwNlLpJaa5Y%2FHJ8H%2Byf4Qa3NsQTKSg4eSRyflY3YqlHNZBqA0USoF7umlH5zTt`

        // `http://salesforce-iframe-back-stag-ru.herokuapp.com/Games/RecommendIt/Next?game=${
        //   this.literals.gameId ? this.literals.gameId : 2
        // }`
      )
      .then(response => {
        this.currentQuestion = response.data;
        this.questionId = response.data.id;
        this.hasNext = response.data.hasNext;
        this.firstQuestion = response.data.id;
        this.buyerQuestion = response.data.questionTitle;
        this.correctAnswer = response.data.correctAnswer.trim();
        this.answer[0] = response.data.correctAnswer;
        this.answer[1] = response.data.wrongAnswer;
        this.buyer = response.data.buyer;
        // console.log("second API", response.data);
      });
  },
  computed: {
    smallQuestion: function() {
      return {
        small: this.question.length < 40
      };
    },

    shuffle: function() {
      let suffleAnswer = this.answer;
      let counter = suffleAnswer.length;
      // While there are elements in the array
      while (counter > 0) {
        // Pick a random index
        let index = Math.floor(Math.random() * counter);

        // Decrease counter by 1
        counter--;

        // And swap the last element with it
        let temp = suffleAnswer[counter];
        suffleAnswer[counter] = suffleAnswer[index];
        suffleAnswer[index] = temp;
      }
      return suffleAnswer;
    }
  },
  methods: {
    detectAnswer: function(event) {
      this.checkedAnswer = event.target.innerText;
      if (this.checkedAnswer == this.correctAnswer) {
        event.target.classList.add("correct");
        this.check = !this.check;
      } else {
        event.target.classList.add("incorrect");
        this.check = !this.check;
        this.fails += 1;
      }
    },
    nextLevel: function() {
      let qNextApi = `https://iframe-mta-stage-games.brights.io/RecommendIt/Next?game=${
        this.literals.gameId ? this.literals.gameId : 2
      }&id=${
        this.questionId
      }&QUERY=LDDFnDrHsxDhH5Cfz6Iu4peu4jBptK%2BNxjAh1uThMtsNxuaVG90OUN8GO0t5RADymTWH5VFelL8Ig1R0IGWbIuqGtf4WIb%2Blv2AqMtimNMHypp8DWCrFa%2B3YHWs34%2Fshdb9SaDmPbf%2FlYhqppPbHQkdzA4KP29Xj0k3WXyjYtYRqBejC3hw9NPLcKM2yjTwBmexznskVUM8TD9Zk5W9LdLFGwbu3XFR0OMmxN2Ejvmbtp82yKgReMrW8Y3pr4D6NBcv04m06GoSKHsIs6Ckeg0wS%2BcehiYtlRddi3GsHHJwF3BcD%2BjIk%2Bxo3%2Bz8rmEm5xW31abDQfLElP4OzNlGAedKQZ7oLSL4mPe2ZO6LwpuCC4Zq33zcDtuNKkbhUQr0z5M0UMVJzRTKe%2FQwcbH4Yrjt2dbRGWirNGHcb4mqfL1a45BCjlmDnmWGScUfq3h2wP%2FvS9NNiZFazy%2B0CfVcbRZvPcsjBmGCF2oYASDzs6WXoWrifTfsuDf%2FeGvaoTJUQyFCfogaNU8HXks75nxS5ELOyIeQBvJy1RyQmaKtUZQxyTcQg%2BI9VOnIx%2FwN%2F%2BpfbL9GwNlLpJaa5Y%2FHJ8H%2Byf4Qa3NsQTKSg4eSRyflY3YqlHNZBqA0USoF7umlH5zTt`;
      // let qNextApi = `http://salesforce-iframe-back-stag-ru.herokuapp.com/Games/RecommendIt/Next?game=${
      //   this.literals.gameId ? this.literals.gameId : 2
      // }&id=${this.questionId}`;

      axios.get(qNextApi).then(response => {
        this.currentQuestion = response.data;
        this.questionId = response.data.id;
        this.hasNext = response.data.hasNext;
        this.prevQuestion = response.data.prevId;
        this.buyer = response.data.buyer;
        this.buyerQuestion = response.data.questionTitle;
        this.correctAnswer = response.data.correctAnswer.trim();
        this.answer[0] = response.data.correctAnswer;
        this.answer[1] = response.data.wrongAnswer;
        // console.log("nextlevel API", response.data);
      });
      this.check = !this.check;
    },
    // restartLevel: function() {
    //   let qNextApi = `https://iframe-mta-stage-games.brights.io/RecommendIt/Next?game=${
    //     this.literals.gameId ? this.literals.gameId : 2
    //   }&id=${
    //     this.prevQuestion
    //   }&QUERY=LDDFnDrHsxDhH5Cfz6Iu4peu4jBptK%2BNxjAh1uThMtsNxuaVG90OUN8GO0t5RADymTWH5VFelL8Ig1R0IGWbIuqGtf4WIb%2Blv2AqMtimNMHypp8DWCrFa%2B3YHWs34%2Fshdb9SaDmPbf%2FlYhqppPbHQkdzA4KP29Xj0k3WXyjYtYRqBejC3hw9NPLcKM2yjTwBmexznskVUM8TD9Zk5W9LdLFGwbu3XFR0OMmxN2Ejvmbtp82yKgReMrW8Y3pr4D6NBcv04m06GoSKHsIs6Ckeg0wS%2BcehiYtlRddi3GsHHJwF3BcD%2BjIk%2Bxo3%2Bz8rmEm5xW31abDQfLElP4OzNlGAedKQZ7oLSL4mPe2ZO6LwpuCC4Zq33zcDtuNKkbhUQr0z5M0UMVJzRTKe%2FQwcbH4Yrjt2dbRGWirNGHcb4mqfL1a45BCjlmDnmWGScUfq3h2wP%2FvS9NNiZFazy%2B0CfVcbRZvPcsjBmGCF2oYASDzs6WXoWrifTfsuDf%2FeGvaoTJUQyFCfogaNU8HXks75nxS5ELOyIeQBvJy1RyQmaKtUZQxyTcQg%2BI9VOnIx%2FwN%2F%2BpfbL9GwNlLpJaa5Y%2FHJ8H%2Byf4Qa3NsQTKSg4eSRyflY3YqlHNZBqA0USoF7umlH5zTt`;

    //   axios.get(qNextApi).then(response => {
    //     this.currentQuestion = response.data;
    //     this.questionId = response.data.id;
    //     this.hasNext = response.data.hasNext;
    //     this.prevQuestion = response.data.prevId;
    //     this.buyer = response.data.buyer;
    //     this.buyerQuestion = response.data.questionTitle;
    //     this.correctAnswer = response.data.correctAnswer.trim();
    //     this.answer[0] = response.data.correctAnswer;
    //     this.answer[1] = response.data.wrongAnswer;
    //     // console.log("nextlevel API", response.data);
    //   });
    //   this.check = !this.check;
    //   this.fails -= 1;
    // },
    restartGame: function() {
      let restartApi = `https://iframe-mta-stage-games.brights.io/RecommendIt/Next?game=${
        this.literals.gameId ? this.literals.gameId : 2
      }&QUERY=LDDFnDrHsxDhH5Cfz6Iu4peu4jBptK%2BNxjAh1uThMtsNxuaVG90OUN8GO0t5RADymTWH5VFelL8Ig1R0IGWbIuqGtf4WIb%2Blv2AqMtimNMHypp8DWCrFa%2B3YHWs34%2Fshdb9SaDmPbf%2FlYhqppPbHQkdzA4KP29Xj0k3WXyjYtYRqBejC3hw9NPLcKM2yjTwBmexznskVUM8TD9Zk5W9LdLFGwbu3XFR0OMmxN2Ejvmbtp82yKgReMrW8Y3pr4D6NBcv04m06GoSKHsIs6Ckeg0wS%2BcehiYtlRddi3GsHHJwF3BcD%2BjIk%2Bxo3%2Bz8rmEm5xW31abDQfLElP4OzNlGAedKQZ7oLSL4mPe2ZO6LwpuCC4Zq33zcDtuNKkbhUQr0z5M0UMVJzRTKe%2FQwcbH4Yrjt2dbRGWirNGHcb4mqfL1a45BCjlmDnmWGScUfq3h2wP%2FvS9NNiZFazy%2B0CfVcbRZvPcsjBmGCF2oYASDzs6WXoWrifTfsuDf%2FeGvaoTJUQyFCfogaNU8HXks75nxS5ELOyIeQBvJy1RyQmaKtUZQxyTcQg%2BI9VOnIx%2FwN%2F%2BpfbL9GwNlLpJaa5Y%2FHJ8H%2Byf4Qa3NsQTKSg4eSRyflY3YqlHNZBqA0USoF7umlH5zTt`;
      // let restartApi = `http://salesforce-iframe-back-stag-ru.herokuapp.com/Games/RecommendIt/Next?game=${
      //   this.literals.gameId ? this.literals.gameId : 2
      // }`;

      axios.get(restartApi).then(response => {
        this.currentQuestion = response.data;
        this.questionId = response.data.id;
        this.hasNext = response.data.hasNext;
        this.buyer = response.data.buyer;
        this.buyerQuestion = response.data.questionTitle;
        this.correctAnswer = response.data.correctAnswer.trim();
        this.answer[0] = response.data.correctAnswer;
        this.answer[1] = response.data.wrongAnswer;
        // console.log("nextlevel API", response.data);
      });
      this.check = !this.check;
      this.fails = 0;

      this.$emit("next-point", this.fails);
    },
    theEnd: function() {
      this.hasNext ? this.nextLevel() : this.$emit("end-game", this.fails);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='scss'>
// .start-screen {
//   position: relative;
//   width: 100%;
//   height: 100vh;
//   background-image: url("/img/1-st-screen-light.png");
//   background-position: center center;
//   background-size: cover;
// }

.start-screen {
  background: #e1e9ec;
  background-image: none;
}

.game__wrapper {
  @include position(center);
  width: 100%;
  height: 85%;
  background: #e1e9ec;
  padding: 10px;
  max-height: 588px;
  max-width: 415px;
  @media only screen and (max-width: 320px) {
    height: 90%;
  }
  @include media(t) {
    padding: 10px 30px;

    min-height: 787px;
    max-height: 787px;
    min-width: 767px;
    max-width: 767px;
  }
}

.img__wrapper {
  width: 100%;
  height: 65%;
  position: relative;
  overflow: hidden;
  @include media(t) {
    height: 81%;
  }
}

.stan {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  transition: all 1s ease;
  opacity: 0;
  &.active {
    opacity: 1;
  }
}

.img-bg {
  width: 100%;
  position: absolute;
  @include position(centerX);
  // bottom: 0;
  &.top {
    top: 0;
    z-index: 0;
    @media only screen and (max-width: 320px) {
      top: 10px;
    }
    @include media(t) {
      top: -40px;
    }
  }
  &.bottom {
    bottom: 5px;
    z-index: 5;
  }
}
.img-buyer {
  position: absolute;
  left: 8%;
  // transition: all 0.1s ease;
  width: 0;
  bottom: 22.7%;
  @include media(t) {
    bottom: 24%;
  }
  &.active {
    width: 31%;
    @include media(t) {
      // bottom: 24%;
    }
  }
  &.inactive {
    width: 19.7%;
    // bottom: 23%;
  }
}

.img-seller {
  position: absolute;
  right: 5%;
  bottom: 23.7%;
  @include media(t) {
    bottom: 25.5%;
  }
  &.active {
    width: 31%;
    @include media(t) {
      // bottom: 24.5%;
    }
  }
  &.inactive {
    width: 20%;
    // bottom: 23.5%;
    @include media(t) {
      // bottom: 25%;
    }
  }
}

.buyer-emotion {
  width: 20%;
  position: absolute;
  bottom: 63%;
  left: 6%;
  animation-name: buyer-emotion;
  animation-duration: 1s;
  animation-timing-function: cubic-bezier(1, -1.35, 0, 2.38);
  animation-fill-mode: forwards;
  animation-iteration-count: 1;
  @media only screen and (max-width: 320px) {
    bottom: 59%;
  }
}

@keyframes buyer-emotion {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}

.buyer_question {
  background-image: url("~@/assets/icon/bubble-customer.svg");
  min-width: 60%;
  min-height: 120px;
  height: auto;
  width: auto;
  position: absolute;
  left: 50%;
  bottom: 63%;
  background-position: center bottom;
  background-repeat: no-repeat;
  background-size: 100% 100%;
  // background-color: red;
  padding: 25px 10px 20px 20px;
  color: #fff;
  font-size: 12px;
  line-height: 1.3;
  font-weight: 600;
  transform: translate(-95%, 50%) scale(0);
  transition: all 0.5s ease;
  &.active {
    transform: translate(-50%, 0) scale(1);
  }
  @media only screen and (max-width: 320px) {
    min-height: 100px;
    padding: 20px 10px 50px 15px;
    bottom: 58%;
  }
  @include media(t) {
    padding: 50px 20px 85px 30px;
    min-height: 220px;
    font-size: 22px;
  }
}
.seller_answer {
  min-width: 63%;
  min-height: 120px;
  height: auto;
  width: auto;
  position: absolute;
  left: 50%;
  bottom: 64%;
  background-position: 0 0;
  background-repeat: no-repeat;
  background-size: 100% 100%;
  // background-color: red;
  padding: 15px 10px 20px 15px;
  color: #fff;
  font-size: 12px;
  line-height: 1.3;
  font-weight: 600;
  transform: translate(20%, 50%) scale(0);
  transition: all 0.5s ease;
  &.active {
    transform: translate(-50%, 0) scale(1);
  }
  &.good {
    background-image: url("~@/assets/icon/bubble-seller-green.svg");
  }
  &.bad {
    background-image: url("~@/assets/icon/bubble-seller-red.svg");
  }
  @media only screen and (max-width: 320px) {
    padding: 22px 10px 50px 15px;
    font-size: 10px;
    min-height: 100px;
    bottom: 58%;
  }
  @include media(t) {
    padding: 30px 30px 85px 20px;
    min-height: 220px;
    font-size: 22px;
  }
}

.seller-emotion {
  width: 20%;
  position: absolute;
  bottom: 60%;
  right: 3%;
  animation-name: seller-emotion;
  animation-duration: 1s;
  animation-timing-function: cubic-bezier(1, -1.35, 0, 2.38);
  animation-fill-mode: forwards;
  animation-iteration-count: 1;
  @media only screen and (max-width: 320px) {
    bottom: 59%;
  }
}

@keyframes seller-emotion {
  0% {
    transform: scale(0);
  }
  100% {
    transform: scale(1);
  }
}

.question__wrapper {
  width: 100%;
  height: 48%;
  position: absolute;
  bottom: 0;
  left: 0;
  z-index: 6;
  @include media(t) {
    height: 35%;
  }
}

.question {
  color: #004f80;
  text-align: center;
  font-weight: 600;
  font-size: 15px;
  line-height: 1.2;
  margin-bottom: 10px;
  padding: 0 20px;
  // z-index: 3;
  transition: all 0.5s ease;
  &.small {
    margin: 20px 0;
  }
  &.hide {
    opacity: 0;
  }
  @include media(t) {
    margin: 30px 0;
    font-size: 22px;
    &.small {
      // margin: 30px 0;
      margin: 0 0 30px 0;
    }
  }
}

.question-btn {
  text-transform: none;
  height: auto;
  min-height: 50px;
  width: 90%;
  line-height: 1.6;
  padding: 10px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.2);
  &:first-of-type {
    margin-bottom: 8px;
  }
  &.correct {
    background: #41c9b0;
  }
  &.incorrect {
    background: #df514e;
  }
}

.result__wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  position: absolute;
  width: 90%;
  height: 35%;
  bottom: 5%;
  left: 5%;
  background: #fff;
  z-index: 6;
  border-radius: 6px;
  padding-bottom: 30px;
  @media only screen and (max-width: 320px) {
    padding: 15px;
  }
  @include media(t) {
    height: 26%;
  }
}

.result-text {
  font-size: 22px;
  line-height: 1.2;
  color: #004f80;
  font-weight: 600;
  text-align: center;
  width: 70%;
  margin-bottom: 20px;
  @media only screen and (max-width: 320px) {
    margin-bottom: 10px;
    font-size: 18px;
  }
}

.btn-line {
  display: flex;
  justify-content: center;
  width: 100%;
  .main-btn {
    min-width: unset;
    width: auto;
    padding: 0 20px;
    margin: 0 10px;
    @media only screen and (max-width: 320px) {
      padding: 0 15px;
    }
  }
  .next-question {
    background: #ced2d8;
    color: #616363;
  }
}
.check-wrap {
  display: block;
  width: 65px;
  height: 65px;
  padding: 5px;
  background: #fff;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: -32.5px;
  border-radius: 50%;
  @media only screen and (max-width: 320px) {
    width: 45px;
    height: 45px;
    top: -22.5px;
  }
}
</style>