<template>
  <div class="menu-bar">
    <transition name="slide-up">
      <div :class="{'tabs': true, 'top-shadow': !isSelectedItem}" v-show="showTab" ref="tabs">
        <div class="icon-wrapper" @click="handleSelectTab('navigation')">
          <span class="ebook-icon-menu ebook-icon"></span>
        </div>
        <div class="icon-wrapper" @click="handleSelectTab('location')">
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
    <transition name="slide-up">
        <div :class="{'setting-wrapper': true, 'top-shadow': isSelectedItem}" v-show="isSelectedItem">
          <div class="setting-font-size" v-show="tabFlag === 'fontSize' && fontSizeList.length > 0">
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
          <div class="set-theme" v-show="tabFlag === 'theme' && themeList.length > 0">
            <div v-for="item in themeList" class="theme-item" @click="selectTheme(item)" :key="item.name">
              <div :class="{'theme-background': true, 'selected': theme.name === item.name}" :style="{background: item.style.body.background}"></div>
              <div :class="{'theme-name': true, 'selected': theme.name === item.name}">{{item.name}}</div>
            </div>
          </div>
          <div class="set-location" v-show="tabFlag === 'location'">
            <div class="process-wrapper">
              <input class="process"
                     ref="process"
                     type="range"
                     min="0"
                     max="100"
                     step="1"
                     :value="location"
                     :disabled="loadingLocations"
                     @change="onProcessChange"
                     @input="onProcessInput"/>
              <div class="status">{{loadingLocations ? '加载中' : `${location}%`}}</div>
            </div>
          </div>
        </div>
      </transition>
    <content-view
      :loading="loadingNavigations"
      v-show="tabFlag === 'navigation'"
      :navigation="navigation"
      @select-item="selectHref"/>
    <transition name="fade">
      <div class="content-mask" v-show="tabFlag === 'navigation'" @click="hideContent">
      </div>
    </transition>
  </div>
</template>

<script>
  import ContentView from "@/components/ContentView";
  export default {
    name: "tabs",
    components: {ContentView},
    props: {
      show: {
        type: Boolean,
        required: true
      },
      loadingLocations: {
        type: Boolean,
        required: true
      },
      loadingNavigations: {
        type: Boolean,
        required: true
      },
      navigation: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    data() {
      return {
        showTab: false,
        tabList: ['fontSize', 'theme', 'location', 'navigation'],
        tabFlag: '',
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
        },
        location: 0
      }
    },
    computed: {
      isSelectedItem() {
        return this.tabFlag && this.tabFlag !== 'navigation'
      }
    },
    methods: {
      handleSelectTab(type) {
        this.tabFlag = type
      },
      selectFontSize (item) {
        this.fontSize = item
        this.$emit('set-font-size', this.fontSize.fontSize)
      },
      selectTheme(theme) {
        this.theme = theme
        this.$emit('set-theme', this.theme)
      },
      onProcessChange(event) {
        const value = event.target.value
        this.$emit('set-location', value)
      },
      onProcessInput(event) {
        this.location = event.target.value
        this.$refs.process.style.backgroundSize = `${location}% 100%`
      },
      hideContent() {
        this.tabFlag = ''
      },
      selectHref (href) {
        this.tabFlag = ''
        this.$emit('jump-to', href)
      }
    },
    watch: {
      show (val) {
        this.showTab = val
        if (!val) {
          this.tabFlag = ''
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
        height: 100%;
        @include center();
        flex-direction: row;
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
              border: px2rem(1) solid #ff0000;
            }
          }
          .theme-name {
            font-size: px2rem(20);
            color: #ccc;
            text-align: center;
            &.selected {
              color: #000;
              font-weight: 400;
            }
          }
        }
      }
      .set-location {
        height: 100%;
        @include center();
        flex-direction: row;
        .process-wrapper {
          height: 100%;
          width: 100%;
          @include center();
          flex-direction: column;
          padding: 0 px2rem(30);
          box-sizing: border-box;
          .process {
            width: 100%;
            -webkit-appearance: none;
            height: px2rem(2);
            background: -webkit-linear-gradient(#333,#999) no-repeat, #ddd;
            background-size: 0 100%;
            margin-bottom: px2rem(15);
            &:focus {
              outline: none;
            }
            &::-webkit-slider-thumb {
              -webkit-appearance: none;
              height: px2rem(20);
              width: px2rem(20);
              border-radius: 50%;
              background: #fff;
              box-shadow: 0 4px 4px 0 rgba(0,0,0,0.15);
              bordeer: px2rem(1) solid #ddd;
            }
          }
          .status {
            text-align: center;
            font-size: px2rem(12);
          }
        }
      }
    }
    .top-shadow {
      box-shadow: 0 px2rem(-8) px2rem(8) rgba(0,0,0,0.15);
    }
    .content-mask {
      position: fixed;
      top: 0;
      left: 0;
      left: 0;
      z-index: 110;
      display: flex;
      height: 100%;
      width: 100%;
      background-color: rgba(51,51,51,.8);
    }
  }
  input[type="range"] {
    height: 10px;
    width: 100%;
  }
</style>
