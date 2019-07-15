<template>
  <div class="start-screen" v-if="final">
    <div class="game__wrapper">
      <div class="info__wrapper">
        <template v-if="fails == 0">
          <img src="~@/assets/icon/result-good.svg" alt="emotion-image" class="result-image" />
          <p class="result-text">{{literals.recommendit_win}}</p>
          <a
            :href="literals.recommedit_homeurl"
            class="main-btn result-btn"
          >{{literals.recommendit_mainpage}}</a>
        </template>

        <template v-else>
          <img src="~@/assets/icon/result-bad.svg" alt="emotion-image" class="result-image" />
          <p class="result-text">{{literals.recommendit_loose}}</p>
          <button
            class="main-btn result-btn"
            @click="$emit('next-point')"
          >{{literals.recommendit_restart}}</button>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "FinalScreen",
  props: {
    final: {
      type: Boolean,
      default: false
    },
    literals: {
      type: Object
    },
    fails: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {};
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='scss'>
// .final-screen {
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
  height: 80%;
  background: #e1e9ec;
  padding: 10px;
  max-height: 588px;
  @include media(t) {
    min-height: 767px;
    max-height: 767px;
    min-width: 690px;
    max-width: 690px;
  }
}
.info__wrapper {
  @include position(center);
  width: 90%;
  height: 90%;
  background: #fff;
  border-radius: 5px;
  padding: 30px 20px;
  box-shadow: 0px 10px 12px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
}

.result-image {
  @include position(centerX);
  top: 25%;
}

.result-text {
  color: #004f80;
  font-size: 22px;
  line-height: 1.2;
  text-align: center;
  font-weight: 600;
  margin: 15px 0 50px;
}
</style>