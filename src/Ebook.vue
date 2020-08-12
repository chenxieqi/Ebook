<template>
  <div class="ebook">
      <transition name="slide-down">
        <div class="title-wapper" v-show="ifTitleAndMenuShow">
          <div class="left">
            <span class="icon-back icon"></span>
          </div>
          <div class="right">
            <div class="icon-wapper">
              <span class="icon-cart icon"></span>
            </div>
            <div class="icon-wapper">
              <span class="icon-person icon"></span>
            </div>
            <div class="icon-wapper">
              <span class="icon-more icon"></span>
            </div>
          </div>
        </div>
      </transition>
      <div class="read-wapper">
          <div id="read"></div>
          <div class="mask">
            <div class="left" @click="prevPage"></div>
            <div class="center" @click="toggleTitleAndMenu"></div>
            <div class="right" @click="nextPage"></div>
          </div>
      </div>
      <transition name="slide-up">
        <div class="menu-wapper" v-show="ifTitleAndMenuShow">
          <div class="icon-wapper">
            <span class="icon-menu icon"></span>
          </div>
          <div class="icon-wapper">
            <span class="icon-progress icon"></span>
          </div>
          <div class="icon-wapper">
            <span class="icon-bright icon"></span>
          </div>
          <div class="icon-wapper">
            <span class="icon-a icon">A</span>
          </div>
        </div>
      </transition>
  </div>
</template>

<script>
import Epub from 'epubjs'
const DOWNLOAD_URL = './static/2018_Book_AgileProcessesInSoftwareEngine.epub'
global.ePub = Epub
export default {
  data() {
    return {
      ifTitleAndMenuShow: false
    }
  },
  mounted() {
    this.showEpub()
  },
  methods: {
    toggleTitleAndMenu() {
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
    },
    prevPage() {
      // redition.prev
      if (this.rendition) {
        this.rendition.prev()
      }
    },
    nextPage() {
      // redition.next
      if (this.rendition) {
        this.rendition.next()
      }
    },
    // ebook
    showEpub() {
      // create book
      this.book = new Epub(DOWNLOAD_URL)
      // create Rendition,using book.rendTo
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      // using rendition.display to display book
      this.rendition.display()
    }
  }
}
</script>

<style lang='scss' scoped>
@import 'assets/styles/global';
.ebook{
  position: relative;
  .title-wapper {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 101;
    display: flex;
    width: 100%;
    height: px2rem(48);
    background:white;
    box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, .15);
    .left {
      flex: 0 0 px2rem(60);
      @include center;
    }
    .right {
      flex: 1;
      display: flex;
      justify-content: flex-end;
      .icon-wapper {
        flex: 0 0 px2rem(40);
        @include center;
      }
    }
  }
  .read-wapper {
    .mask{
      position:absolute;
      top: 0;
      left: 0;
      z-index: 100;
      display: flex;
      width: 100%;
      height: 100%;
      .left{
        flex: 0 0 px2rem(100);
      }
      .center{
        flex: 1;
      }
      .right{
        flex: 0 0 px2rem(100);
      }
    }
  }
  .menu-wapper {
    position: absolute;
    bottom: 0;
    left: 0;
    z-index: 101;
    display: flex;
    width: 100%;
    height: px2rem(48);
    background:white;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
    .icon-wapper {
      flex: 1;
      @include center;
      .icon-progress {
        font-size: px2rem(28);
      }
      .icon-bright {
        font-size: px2rem(24);
      }
    }
  }
}
</style>
