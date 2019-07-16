<template>
  <div id="app" :style="style">
    <StartScreen
      :seller="seller"
      :literals="literals"
      :start="start"
      @next-point="next = !next, start = !start"
    />
    <Q1Screen
      :seller="seller"
      :literals="literals"
      :next="next"
      @end-game="endGame"
      :key="componentKey"
      @next-point="next = !next, start = !start"
    />
    <FinalScreen
      :fails="fails"
      :literals="literals"
      :final="final"
      @next-point="next = !next, final = !final"
    />
  </div>
</template>

<script>
import StartScreen from "./components/StartScreen.vue";
import FinalScreen from "./components/FinalScreen.vue";
import Q1Screen from "./components/Q1Screen.vue";
import axios from "axios";

export default {
  name: "app",
  components: {
    StartScreen,
    FinalScreen,
    Q1Screen
  },
  props: {
    fails: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      literals: {},
      start: true,
      next: false,
      final: false,
      componentKey: 0,
      background: "",
      seller: {}
    };
  },
  computed: {
    style() {
      return `background-image: url(${this.background})`;
      // return `background-image: url("/img/1-st-screen-light.png")`;
    }
  },
  mounted() {
    this.startGame();
  },
  methods: {
    startGame: function() {
      //get gameID from query parametr
      const url = new URL(window.location.href);
      this.literals.gameId = url.searchParams.get("game");
      //get data from start API
      axios
        .get(
          `https://iframe-mta-stage-games.brights.io/RecommendIt/Start?game=${
            this.literals.gameId ? this.literals.gameId : 2
          }&QUERY=LDDFnDrHsxDhH5Cfz6Iu4peu4jBptK%2BNxjAh1uThMtsNxuaVG90OUN8GO0t5RADymTWH5VFelL8Ig1R0IGWbIuqGtf4WIb%2Blv2AqMtimNMHypp8DWCrFa%2B3YHWs34%2Fshdb9SaDmPbf%2FlYhqppPbHQkdzA4KP29Xj0k3WXyjYtYRqBejC3hw9NPLcKM2yjTwBmexznskVUM8TD9Zk5W9LdLFGwbu3XFR0OMmxN2Ejvmbtp82yKgReMrW8Y3pr4D6NBcv04m06GoSKHsIs6Ckeg0wS%2BcehiYtlRddi3GsHHJwF3BcD%2BjIk%2Bxo3%2Bz8rmEm5xW31abDQfLElP4OzNlGAedKQZ7oLSL4mPe2ZO6LwpuCC4Zq33zcDtuNKkbhUQr0z5M0UMVJzRTKe%2FQwcbH4Yrjt2dbRGWirNGHcb4mqfL1a45BCjlmDnmWGScUfq3h2wP%2FvS9NNiZFazy%2B0CfVcbRZvPcsjBmGCF2oYASDzs6WXoWrifTfsuDf%2FeGvaoTJUQyFCfogaNU8HXks75nxS5ELOyIeQBvJy1RyQmaKtUZQxyTcQg%2BI9VOnIx%2FwN%2F%2BpfbL9GwNlLpJaa5Y%2FHJ8H%2Byf4Qa3NsQTKSg4eSRyflY3YqlHNZBqA0USoF7umlH5zTt`
        )
        .then(response => {
          this.literals = response.data.literals;
          this.background = response.data.backgroundPath;
          this.seller = response.data.seller;
          this.literals.gameId = response.data.gameId;
          this.literals.win = response.data.newPlayer;
          // console.log("first API", this.literals);
        });
    },

    //get data for End game state

    endGame: function(item) {
      (this.final = !this.final), (this.next = !this.next);
      this.componentKey += 1;
      this.fails = item;

      //item is count of true answers
      if (item == 0) {
        axios.post(
          `https://iframe-mta-stage-games.brights.io/RecommendIt/WinGame?game=${
            this.literals.gameId ? this.literals.gameId : 2
          }&QUERY=LDDFnDrHsxDhH5Cfz6Iu4peu4jBptK%2BNxjAh1uThMtsNxuaVG90OUN8GO0t5RADymTWH5VFelL8Ig1R0IGWbIuqGtf4WIb%2Blv2AqMtimNMHypp8DWCrFa%2B3YHWs34%2Fshdb9SaDmPbf%2FlYhqppPbHQkdzA4KP29Xj0k3WXyjYtYRqBejC3hw9NPLcKM2yjTwBmexznskVUM8TD9Zk5W9LdLFGwbu3XFR0OMmxN2Ejvmbtp82yKgReMrW8Y3pr4D6NBcv04m06GoSKHsIs6Ckeg0wS%2BcehiYtlRddi3GsHHJwF3BcD%2BjIk%2Bxo3%2Bz8rmEm5xW31abDQfLElP4OzNlGAedKQZ7oLSL4mPe2ZO6LwpuCC4Zq33zcDtuNKkbhUQr0z5M0UMVJzRTKe%2FQwcbH4Yrjt2dbRGWirNGHcb4mqfL1a45BCjlmDnmWGScUfq3h2wP%2FvS9NNiZFazy%2B0CfVcbRZvPcsjBmGCF2oYASDzs6WXoWrifTfsuDf%2FeGvaoTJUQyFCfogaNU8HXks75nxS5ELOyIeQBvJy1RyQmaKtUZQxyTcQg%2BI9VOnIx%2FwN%2F%2BpfbL9GwNlLpJaa5Y%2FHJ8H%2Byf4Qa3NsQTKSg4eSRyflY3YqlHNZBqA0USoF7umlH5zTt`
        );
      }
    }
  }
};
</script>

<style lang="scss">
#app {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100vh;
  background-position: center center;
  background-size: cover;
}

.start-screen {
  position: relative;
  width: 100%;
  height: 100%;
  // position: relative;
  // width: 100%;
  // height: 100vh;
  // background-image: url("/img/1-st-screen-light.png");
  // background-position: center center;
  // background-size: cover;
}
</style>
