<template>
  <transition name="slide-down">
    <div class="top-bar" v-show="show">
      <div class="left">
        <span class="ebook-icon-back ebook-icon"></span>
      </div>
      <div class="right">
        <div class="icon-wrapper">
          <span class="ebook-icon-cart ebook-icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="ebook-icon-person ebook-icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="ebook-icon-more ebook-icon"></span>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  export default {
    name: "top-bar",
    props: {
      show: {
        type: Boolean,
        required: true
      }
    },
    data() {
      return {}
    },
    methods: {}
  }
</script>

<style scoped lang="scss">
  @import "../assets/global";
  .top-bar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    height: px2rem(48);
    z-index: 101;
    background-color: white;
    box-shadow: 0 px2rem(8) px2rem(8) rgba(0,0,0,0.15);
    display: flex;
    .left {
      flex: 0 0 px2rem(60)
    }
    .right {
      flex: 1;
      display: flex;
      justify-content: flex-end;
      .icon-wrapper {
        flex: 0 0 px2rem(40);
      }
    }
    &.slide-down-enter-active, &.slide-down-leave-active, {
      transition: all 300ms linear;
    }
    &.slide-down-enter, &.slide-down-leave-to {
      transform: translate3d(0,-100%,0);
    }
    &.slide-down-enter-to, &.slide-down-leave {
      transform: translate3d(0,0,0);
    }
  }
</style>
