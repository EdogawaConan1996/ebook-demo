<template>
    <transition name="slide-right">
      <div class="content-view">
        <div class="content">
          <div class="content-wrapper" v-if="navigation && navigation.toc.length > 0">
            <div class="content-item" v-for="(item,index) in navigation.toc" :key="item.id" @click="selectItem(item)">
              <span class="text">{{item.label}}</span>
            </div>
          </div>
          <div class="empty" v-else>加载中......</div>
        </div>
      </div>
    </transition>
</template>

<script>
  export default {
    name: "ContentView",
    props: {
      navigation: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    data() {
      return {}
    },
    methods: {
      selectItem(item) {
        this.$emit('select-item', item.href)
      }
    }
  }
</script>

<style scoped lang="scss">
  @import "../assets/global";
  .content-view {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    width: 70%;
    background-color: #ffffff;
    z-index: 120;
    .content {
      height: 100%;
      width: 100%;
      @include center();
      .content-wrapper {
        height: 100%;
        width: 100%;
        .content-item {
          height: px2rem(60);
          padding: px2rem(6) px2rem(20);
          border-bottom: px2rem(1) solid #ccc;
          box-sizing: border-box;
          .text {
            font-size: px2rem(14);
            color: #333;
          }
        }
      }
      .empty {
        font-size: px2rem(16);
        color: #333;
      }
    }
  }
</style>
