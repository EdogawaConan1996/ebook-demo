<template>
  <div class="EBook">
    <top-bar :show="showTitleAndMenu"/>
    <div id="book-wrapper"></div>
    <tabs :show="showTitleAndMenu" @set-font-size="handleSetFontSize"/>
  </div>
</template>

<script>
import Epub from 'epubjs'
import TopBar from "@/components/TopBar";
import Tabs from "@/components/Tabs";

const DOWMLOAD_URL = 'http://39.97.112.165:8000/2018_Book_AgileProcessesInSoftwareEngine.epub'
export default {
  name: "EBook",
  components: {
    Tabs,
    TopBar

  },
  data () {
    return {
      book: null,
      rendition: null,
      themes: null,
      touchStartX: 0,
      touchStartY: 0,
      touchStartTimeStamp: 0,
      showTitleAndMenu: false
    }
  },
  methods: {
    // 电子书的解析和渲染
    showEPub () {
      // 生成Ebook
      this.book = new Epub(DOWMLOAD_URL)
      // 生成Rendition, 通过Book.renderTo方法
      this.rendition = this.book.renderTo('book-wrapper', {
        width: window.innerWidth,
        height: window.innerHeight,
        method: 'default'
      })
      this.themes = this.rendition.themes
      this.themes.fontSize('18px')
      // 通过Rendition.display生成电子书
      this.rendition.display()
      // 绑定touchstart和touchend事件
      this.rendition.on('touchstart', (event) => {
        this.touchStartX = event.changedTouches[0].clientX
        this.touchStartTimeStamp = event.timeStamp
      })
      this.rendition.on('touchend', (event) => {
        const touchEndX = event.changedTouches[0].clientX
        const touchEndTimeStamp = event.timeStamp
        const moveDistance = touchEndX - this.touchStartX
        const moveTimeStamp = touchEndTimeStamp - this.touchStartTimeStamp
        if (Math.abs(moveDistance) < 50 || moveTimeStamp > 500) {
          return
        }
        if (moveDistance > 50) {
          this.prevPage()
        } else if (moveDistance < -50) {
          this.nextPage()
        }
      })
      this.rendition.on('click', () => {
        this.showTitleAndMenu = !this.showTitleAndMenu
      })
    },
    prevPage() {
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    nextPage() {
      if (this.rendition) {
        this.rendition.next()
      }
    },
    handleSetFontSize(fontSize) {
      if (this.themes) {
        this.themes.fontSize(fontSize + 'px')
      }
    }

  },
  mounted() {
    this.showEPub()
  }
}
</script>

<style scoped lang="scss">
  @import "../assets/global";
  .EBook {
    position: relative;
    .menu-wrapper {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      height: px2rem(48);
      z-index: 101;
      background-color: white;
      box-shadow: 0 px2rem(-8) px2rem(8) rgba(0,0,0,0.15);
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
        transition: all 500ms linear;
      }
      &.slide-up-enter, &.slide-up-leave-to {
        transform: translate3d(0,100%,0);
      }
      &.slide-up-enter-to, &.slide-up-leave {
        transform: translate3d(0,0,0);
      }
    }
  }
</style>
