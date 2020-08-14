<template>
  <div class="ebook">
      <title-bar :ifTitleAndMenuShow="ifTitleAndMenuShow"></title-bar>
      <div class="read-wrapper">
          <div id="read"></div>
          <div class="mask">
            <div class="left" @click="prevPage"></div>
            <div class="center" @click="toggleTitleAndMenu"></div>
            <div class="right" @click="nextPage"></div>
          </div>
      </div>
      <menu-bar :ifTitleAndMenuShow="ifTitleAndMenuShow"
      :fontSizeList="fontSizeList"
      :defaultFontSize="defaultFontSize"
      @setFontSize="setFontSize"
      :themeList="themeList"
      :defaultTheme="defaultTheme"
      @themeSelect="themeSelect"
      ref="MenuBar"></menu-bar>
  </div>
</template>

<script>
import TitleBar from '@/components/TitleBar'
import MenuBar from '@/components/MenuBar'
import Epub from 'epubjs'
const DOWNLOAD_URL = './static/2018_Book_AgileProcessesInSoftwareEngine.epub'
global.ePub = Epub
export default {
  components: {
    TitleBar,
    MenuBar
  },
  data() {
    return {
      ifTitleAndMenuShow: false,
      fontSizeList: [
        {fontSize: 12},
        {fontSize: 14},
        {fontSize: 16},
        {fontSize: 18},
        {fontSize: 20},
        {fontSize: 22},
        {fontSize: 24}
      ],
      defaultFontSize: 16,
      themeList: [
        {
          name: 'default',
          style: {
            body: {
              'color': '#000', 'background': '#fff'
            }
          }
        },
        {
          name: 'eye',
          style: {
            body: {
              'color': '#000', 'background': '#ceeaba'
            }
          }
        },
        {
          name: 'night',
          style: {
            body: {
              'color': '#fff', 'background': '#000'
            }
          }
        },
        {
          name: 'gold',
          style: {
            body: {
              'color': '#000', 'background': 'rgb(241, 236, 226)'
            }
          }
        }
      ],
      defaultTheme: 0
    }
  },
  mounted() {
    this.showEpub()
  },
  methods: {
    themesRegister() {
      if (this.themes) {
        this.themeList.forEach(theme => {
          this.themes.register(theme.name, theme.style)
        }
        )
      }
    },
    themeSelect(index) {
      if (this.themes) {
        this.themes.select(this.themeList[index].name)
      }
    },
    setFontSize(fontSize) {
      this.defaultFontSize = fontSize
      if (this.themes) {
        this.themes.fontSize(fontSize + 'px')
      }
    },
    toggleTitleAndMenu() {
      this.ifTitleAndMenuShow = !this.ifTitleAndMenuShow
      if (!this.ifTitleAndMenuShow) {
        this.$refs.MenuBar.hideFontSetting()
      }
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
      this.themes = this.rendition.themes
      this.setFontSize(this.defaultFontSize)
      // redition.themes.register(name, style)
      // redition.themes.select(name)
      this.themesRegister()
    }
  }
}
</script>

<style lang='scss' scoped>
@import 'assets/styles/global';
.ebook{
  position: relative;

  .read-wrapper {
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

}
</style>
