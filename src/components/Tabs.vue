<template>
  <div class="menu-bar">
    <transition name="slide-up">
      <div :class="{'tabs': true, 'top-shadow': !isSelectedItem}" v-show="showTab" ref="tabs">
        <div class="icon-wrapper">
          <span class="ebook-icon-menu ebook-icon"></span>
        </div>
        <div class="icon-wrapper">
          <span class="ebook-icon-progress ebook-icon"></span>
        </div>
        <div class="icon-wrapper" @click="handleSelectTab('theme')">
          <span class="ebook-icon-bright ebook-icon"></span>
        </div>
        <div class="icon-wrapper" @click="handleSelectTab('fontSize')">
          <span class="ebook-icon-a ebook-icon">A</span>
        </div>
      </div>
    </transition>
    <transition name="slide-up" key="fontSize">
        <div :class="{'setting-wrapper': true, 'top-shadow': isSelectedItem}" v-show="isSelectedItem">
          <div class="setting-font-size" v-show="showFontSizeSet && fontSizeList.length > 0">
            <div class="preview" :style="{fontSize: `${fontSizeList[0].fontSize}px`}">A</div>
            <div class="select-wrapper">
              <template v-for="item in fontSizeList">
                <div class="line"></div>
                <div :key="item.fontSize" class="point-wrapper" @click="selectFontSize(item)">
                  <div class="point" v-show="item.fontSize === fontSize.fontSize">
                    <div class="small-point"></div>
                  </div>
                </div>
                <div class="line"></div>
              </template>
            </div>
            <div class="preview" :style="{fontSize: `${fontSizeList[fontSizeList.length - 1].fontSize}px`}">A</div>
          </div>
          <div class="set-theme" v-show="showThemeSet && themeList.length > 0">
            <div v-for="item in themeList" class="theme-item" @click="selectTheme(item)" :key="item.name">
              <div :class="{'theme-background': true, 'selected': theme.name === item.name}" :style="{background: item.style.body.background}"></div>
              <div :class="{'theme-name': true, 'selected': theme.name === item.name}">{{item.name}}</div>
            </div>
          </div>
        </div>
      </transition>
  </div>
</template>

<script>
  export default {
    name: "tabs",
    props: {
      show: {
        type: Boolean,
        required: true
      }
    },
    data() {
      return {
        showTab: false,
        showFontSizeSet: false,
        showThemeSet: false,
        fontSizeList: [
          {fontSize: 12},
          {fontSize: 14},
          {fontSize: 16},
          {fontSize: 18},
          {fontSize: 20},
          {fontSize: 22},
          {fontSize: 24},
        ],
        fontSize: {fontSize: 18},
        themeList: [
          {
            name: 'default',
            style: {
              body: {'color': '#000', 'background': '#fff'}
            }
          },
          {
            name: 'eye',
            style: {
              body: {'color': '#000', 'background': '#ceeaba'}
            }
          },
          {
            name: 'night',
            style: {
              body: {'color': '#fff', 'background': '#000'}
            }
          },
          {
            name: 'gold',
            style: {
              body: {'color': '#000', 'background': 'rgb(241,236,226)'}
            }
          }
        ],
        theme: {
          name: 'default',
          style: {
            body: {'color': '#000', 'background': '#fff'}
          }
        }
      }
    },
    computed: {
      isSelectedItem() {
        return this.showFontSizeSet || this.showThemeSet
      }
    },
    methods: {
      handleSelectTab(type) {
        switch (type) {
          case 'fontSize': this.showFontSizeSet = !this.showFontSizeSet; break;
          case 'theme': this.showThemeSet = !this.showThemeSet; break;
        }
      },
      selectFontSize (item) {
        this.fontSize = item
        this.$emit('set-font-size', this.fontSize.fontSize)
      },
      selectTheme(theme) {
        this.theme = theme
        this.$emit('set-theme', this.theme)
      }
    },
    watch: {
      show (val) {
        this.showTab = val
        if (!val) {
          this.showFontSizeSet = false
          this.showThemeSet = false
        }
      }
    },
    mounted() {
      this.selectFontSize(this.fontSize)
      this.selectTheme(this.theme)
    }
  }
</script>

<style scoped lang="scss">
  @import "../assets/global";
  .menu-bar {
    position: relative;
    .tabs {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      height: px2rem(48);
      z-index: 101;
      background-color: white;
      display: flex;
      .icon-wrapper {
        flex: 1;
        @include center();
        .ebook-icon-progress {
          font-size: px2rem(24);
        }
        .ebook-icon-bright {
          font-size: px2rem(24);
        }
      }
      &.slide-up-enter-active, &.slide-up-leave-active{
        transition: all 300ms linear;
      }
      &.slide-up-enter, &.slide-up-leave-to {
        transform: translate3d(0,100%,0);
      }
      &.slide-up-enter-to, &.slide-up-leave {
        transform: translate3d(0,0,0);
      }
    }
    .setting-wrapper {
      position: absolute;
      z-index: 99;
      bottom: px2rem(48);
      left: 0;
      right: 0;
      height: px2rem(60);
      background-color: #ffffff;
      .setting-font-size {
        display: flex;
        flex-direction: row;
        height: 100%;
        .preview {
          flex: 0 0 px2rem(40);
          @include center()
        }
        .select-wrapper {
          flex: 1;
          @include center();
          .line {
            flex: 1;
            height: 0;
            border-top: px2rem(1) solid #ccc;
            &:first-child,&:last-child {
              border: none;
            }
          }
          .point-wrapper {
            position: relative;
            flex: 0 0 0;
            width: 0;
            height: px2rem(7);
            border-left: px2rem(1) solid #ccc;
            .point {
              position: absolute;
              top: px2rem(-8);
              left: px2rem(-10);
              width: px2rem(20);
              height: px2rem(20);
              border-radius: 50%;
              background-color: white;
              border: px2rem(1) solid #ccc;
              box-shadow: 0 px2rem(4) px2rem(4) rgba(0,0,0,.15);
              @include center();
              .small-point {
                width: px2rem(5);
                height: px2rem(5);
                background-color: #000;
                border-radius: 50%;
              }
            }
          }
        }
      }
      .set-theme {
        display: flex;
        flex-direction: row;
        height: 100%;
        @include center();
        .theme-item {
          flex: 1;
          padding: 0 px2rem(10);
          .theme-background {
            height: px2rem(20);
            width: 100%;
            margin-bottom: px2rem(7);
            border: 1px solid #ccc;
            background-color: #000;
            &.selected {
              border: px2rem(2) solid #ff0000;
            }
          }
          .theme-name {
            font-size: px2rem(20);
            color: #333;
            text-align: center;
            &.selected {
              color: #000;
              font-weight: 400;
            }
          }
        }
      }
      &.slide-up-enter-active, &.slide-up-leave-active{
        transition: all 300ms linear;
      }
      &.slide-up-enter, &.slide-up-leave-to {
        transform: translate3d(0,100%,0);
      }
      &.slide-up-enter-to, &.slide-up-leave {
        transform: translate3d(0,0,0);
      }
    }
    .top-shadow {
      box-shadow: 0 px2rem(-8) px2rem(8) rgba(0,0,0,0.15);
    }
  }
</style>
