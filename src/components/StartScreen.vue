<template>
  <div class="start-screen" v-if="start">
    <div class="game__wrapper">
      <div class="img__wrapper">
        <img src="~@/assets/icon/start-background.svg" alt class="img-bg" />
        <div class="seller-think">
          <p>{{literals.recommendit_challenge}}</p>
        </div>

        <!-- <img src="~@/assets/icon/seller-active.svg" alt class="img-seller" /> -->
        <img :src="seller.firstState" alt class="img-seller" />
      </div>

      <p class="pregame__text">{{literals.recommendit_introtext}}</p>

      <button @click="$emit('next-point')" class="main-btn">{{literals.recommendit_beginbutton}}</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "StartScreen",
  props: {
    start: {
      type: Boolean,
      default: true
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
      info: null
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='scss'>
.game__wrapper {
  @include position(center);
  width: 90%;
  height: 75%;
  max-height: 523px;
  background: #fff;
  border-radius: 5px;
  padding: 30px 20px;
  @media only screen and (max-width: 320px) {
    padding: 10px;
    height: 85%;
  }
  @include media(t) {
    min-height: 767px;
    max-height: 767px;
    min-width: 690px;
    max-width: 690px;
  }
}

.img__wrapper {
  width: 100%;
  height: 55%;
  position: relative;
  overflow: hidden;
  @include media(t) {
    height: 70%;
  }
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
  padding: 20px 27px;
  color: #fff;
  font-size: 12px;
  line-height: 1.3;
  font-weight: 600;
  @include media(t) {
    background-size: 87%;
    min-height: 200px;
    padding: 28px 50px 60px;
    font-size: 26px;
  }
}

.pregame__text {
  color: #004f80;
  font-size: 14px;
  line-height: 1.4;
  text-align: center;
  font-weight: 600;
  margin: 15px 0 25px;
  @include media(t) {
    font-size: 18px;
  }
}
</style>