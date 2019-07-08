<template>
  <div class="start-screen">
    <div class="game__wrapper">
        
      <div class="img__wrapper">
        <img src="~@/assets/icon/start-background.svg" alt="" class="img-bg">
        <div class="seller-think">
          <p>Готовi розпочати?</p>
        </div>

        <img src="~@/assets/icon/seller-active.svg" alt="" class="img-seller">

      </div>


      <p class="pregame__text">
        Проходь гру правильно відповідаючи на питання, та знайомся з рекомендаціями, які допоможуть тобі успішно інформувати повнолітніх курців твоєї торгової точки про найбільш актуальні пропозиції від “Філіп Морріс”
      </p>

      <button class="main-btn">Грати</button>
    </div>
    
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'StartScreen',
  props: {
    
  },
  data() {
    return {
      info: null
    }
  },
  mounted() {
    axios
      .get('https://iframe-mta-stage-games.brights.io/RecommendIt/Start?game=2&QUERY=LDDFnDrHsxDhH5Cfz6Iu4peu4jBptK%2BNxjAh1uThMtsNxuaVG90OUN8GO0t5RADymTWH5VFelL8Ig1R0IGWbIuqGtf4WIb%2Blv2AqMtimNMHypp8DWCrFa%2B3YHWs34%2Fshdb9SaDmPbf%2FlYhqppPbHQkdzA4KP29Xj0k3WXyjYtYRqBejC3hw9NPLcKM2yjTwBmexznskVUM8TD9Zk5W9LdLFGwbu3XFR0OMmxN2Ejvmbtp82yKgReMrW8Y3pr4D6NBcv04m06GoSKHsIs6Ckeg0wS%2BcehiYtlRddi3GsHHJwF3BcD%2BjIk%2Bxo3%2Bz8rmEm5xW31abDQfLElP4OzNlGAedKQZ7oLSL4mPe2ZO6LwpuCC4Zq33zcDtuNKkbhUQr0z5M0UMVJzRTKe%2FQwcbH4Yrjt2dbRGWirNGHcb4mqfL1a45BCjlmDnmWGScUfq3h2wP%2FvS9NNiZFazy%2B0CfVcbRZvPcsjBmGCF2oYASDzs6WXoWrifTfsuDf%2FeGvaoTJUQyFCfogaNU8HXks75nxS5ELOyIeQBvJy1RyQmaKtUZQxyTcQg%2BI9VOnIx%2FwN%2F%2BpfbL9GwNlLpJaa5Y%2FHJ8H%2Byf4Qa3NsQTKSg4eSRyflY3YqlHNZBqA0USoF7umlH5zTt')
      .then(response => (this.info = response.data));
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
  width: 90%;
  height: 75%;
  background: #fff;
  border-radius: 5px;
  padding: 30px 20px;
}

.img__wrapper {
  width: 100%;
  height: 55%;
  position: relative;
  overflow: hidden;
}

.img-bg {
  width: 100%;
  position: absolute;
  @include position(centerX);
  bottom: 0;
}
.img-seller {
  position: absolute;
  width: 45%;
  bottom: 2%;
  right: 5%;
}

.seller-think {
  width: 50%;
  min-height: 100px;
  position: absolute;
  bottom: 60%;
  left: 20%;
  background-image: url("~@/assets/icon/bubble-start.svg");
  background-repeat: no-repeat;
  background-position: center bottom;
  background-size: 90%;
  padding: 20px 40px;
  color: #fff;
  font-size: 12px;
  line-height: 1.3;
  font-weight: 600;
}


.pregame__text {
  color: #004F80;
  font-size: 14px;
  line-height: 1.4;
  text-align: center;
  font-weight: 600;
  margin: 15px 0 25px;
}


</style>