<template>
  <div class="start-screen">
    <div class="game__wrapper">
        
      <div class="img__wrapper">
        <img src="~@/assets/icon/dialog-background.svg" alt="" class="img-bg">
        <div class='think__wrapper'>
          <p class='buyer_question'>{{ buyerQuestion }}</p>
          <p :class="[thinkClass, 'seller_answer']">{{ buyerQuestion }}</p>
        </div>

        <img src="~@/assets/icon/man-active.svg" alt="" class="img-buyer">
        <img v-if='check' src="~@/assets/icon/seller-inactive.svg" alt="" class="img-seller inactive">
        <img v-else src="~@/assets/icon/seller-active.svg" alt="" class="img-seller active">

      </div>

      <div class="question__wrapper">
        <p class='question' :class="{correct: correctAnswer}">{{ question }}</p>
        <button class="question-btn main-btn">{{ shuffle[0] }}</button>
        <button @click='check = !check' class="question-btn main-btn">{{ shuffle[1] }}</button>
      </div>

    </div>
    
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Q1Screen',
  props: {
    },
  data() {
    return {
      info: null,
      question: 'Що Ви вiдповісте покупцевi  ?',
      answer: [
        'Зверніть увагу на новий WINSTON. Капсула із нотками цитрусових фруктів',
        'Зверніть увагу на новий Philip Morris Novel Mix Sun. Капсула із нотками цитрусових фруктів'
      ],
      check: true,
      buyerQuestion: 'А що у Вас є із фруктовою капсулою?',
      correctAnswer: true
    }
  },
  mounted() {
    axios
      .get('https://iframe-mta-stage-games.brights.io/RecommendIt/Start?game=2&QUERY=LDDFnDrHsxDhH5Cfz6Iu4peu4jBptK%2BNxjAh1uThMtsNxuaVG90OUN8GO0t5RADymTWH5VFelL8Ig1R0IGWbIuqGtf4WIb%2Blv2AqMtimNMHypp8DWCrFa%2B3YHWs34%2Fshdb9SaDmPbf%2FlYhqppPbHQkdzA4KP29Xj0k3WXyjYtYRqBejC3hw9NPLcKM2yjTwBmexznskVUM8TD9Zk5W9LdLFGwbu3XFR0OMmxN2Ejvmbtp82yKgReMrW8Y3pr4D6NBcv04m06GoSKHsIs6Ckeg0wS%2BcehiYtlRddi3GsHHJwF3BcD%2BjIk%2Bxo3%2Bz8rmEm5xW31abDQfLElP4OzNlGAedKQZ7oLSL4mPe2ZO6LwpuCC4Zq33zcDtuNKkbhUQr0z5M0UMVJzRTKe%2FQwcbH4Yrjt2dbRGWirNGHcb4mqfL1a45BCjlmDnmWGScUfq3h2wP%2FvS9NNiZFazy%2B0CfVcbRZvPcsjBmGCF2oYASDzs6WXoWrifTfsuDf%2FeGvaoTJUQyFCfogaNU8HXks75nxS5ELOyIeQBvJy1RyQmaKtUZQxyTcQg%2BI9VOnIx%2FwN%2F%2BpfbL9GwNlLpJaa5Y%2FHJ8H%2Byf4Qa3NsQTKSg4eSRyflY3YqlHNZBqA0USoF7umlH5zTt')
      .then(response => (this.info = response.data));
  },
  computed: {
    // thinkClass: function () {
    //   return {
    //     correct: this.answer,
    //     incorrect: !this.answer
    //   }
    // },
    smallQuestion: function () {
      return {
        small: this.question.length < 40 
      }
    }, 
    shuffle: function () {
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
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='scss'>
.start-screen {
  position: relative;
  width: 100%;
  height: 100vh;
  background-image: url("/img/1-st-screen-light.png"); 
  background-position: center center;
  background-size: cover;
}

.game__wrapper {
  @include position(center);
  width: 100%;
  height: 80%;
  background: #E1E9EC;
  padding: 10px;
}

.img__wrapper {
  width: 100%;
  height: 65%;
  position: relative;
  overflow: hidden;
}

.img-bg {
  width: 100%;
  position: absolute;
  @include position(centerX);
  bottom: 0;
}
.img-buyer {
  position: absolute;
  width: 34%;
  bottom: 23%;
  left: 8%;
}

.img-seller {
  position: absolute;
  right: 5%;
  &.active {
    width: 35%;
    bottom: 24%;
  }
  &.inactive {
    width: 23%;
    bottom: 23.5%;
  }
} 
.think__wrapper {
  width: 80%;
  min-height: 100px;
  position: absolute;
  bottom: 70%;
  left: 10%;
  background-image: url("~@/assets/icon/bubble-start.svg");
  background-repeat: no-repeat;
  background-position: center bottom;
  background-size: 90%;
  background: red;
  padding: 20px 40px;
  color: #fff;
  font-size: 12px;
  line-height: 1.3;
  font-weight: 600;
}
.seller-think {
  
}

.question__wrapper {
  width: 100%;
  height: 48%;
  position: absolute;
  bottom: 0;
  left: 0;
}

.question {
  color: #004F80;
  text-align: center;
  font-weight: 600;
  font-size: 15px;
  line-height: 1.2;
  margin-bottom: 10px;
  padding: 0 20px;
  &.small {
    margin: 20px 0;
  }
}

.question-btn {
  text-transform: none;
  height: auto;
  min-height: 50px;
  width: 90%;
  line-height: 1.6;
  padding: 10px;
  &:first-of-type {
    margin-bottom: 8px;
  }
}


</style>