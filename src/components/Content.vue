<template>
  <transition name="slide-right">
      <div class="content">
          <div class="content-wrapper" v-if="bookAvailable">
              <div class="content-item" v-for="(item, index) in navigation.toc" :key="index" @click="jumpTo(item.href)">
                  <span class="text">{{item.label}}</span>
              </div>
          </div>
          <div class="empty" v-else>loading...</div>
      </div>
  </transition>
</template>

<script type="text/ecmascript-6">
export default {
  props: {
    ifShowContent: Boolean,
    bookAvailable: Boolean,
    navigation: Object
  },
  methods: {
    jumpTo(href) {
      this.$emit('jumpTo', href)
      this.$emit('hideSetting')
      this.$emit('hideContent')
    }
  }
}
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
  @import "../assets/styles/global";
  .content {
    position: absolute;
    top: 0;
    left: 0;
    width: 80%;
    height: 100%;
    z-index: 102;
    background:white;
    .content-wrapper {
        width: 100%;
        height: 100%;
        overflow: auto;
        .content-item {
            padding: px2rem(20) px2rem(15);
            border-bottom: px2rem(1) solid #f4f4f4;
            .text {
                font-size: px2rem(14);
                color: #333333;
            }
        }
        .empty {
            width: 100%;
            height: 100%;
            @include center;
            font-size: px2rem(14);
            color: #333333;
        }
    }
  }
</style>
