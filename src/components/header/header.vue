<template>
  <div class="header">
    <div class="header-top">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]">
          </span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
        <div class="support-count" v-if="seller.supports" @click="showDetail">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
    </div>
    <div class="header-bottom" @click="showDetail">
      <span class="notice"></span>
      <span class="notice_text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <div class="detail" v-show="detailShow">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h2 class="name">{{seller.name}}</h2>
          <div class="star-wrapper">
            <star :score="seller.score" :size="48"></star>
          </div>
          <div class="title">
            <span class="line"></span>
              <span class="text">优惠信息</span>
            <span class="line"></span>
          </div>
          <div class="supports-wrapper" v-if="seller.supports && seller.supports.length>0">
            <ul class="supports">
              <li v-for="item in seller.supports" class="support-item">
                <span class="icon" :class="classMap[item.type]"></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
          </div>
          <div class="title">
            <span class="line"></span>
              <span class="text">商家公告</span>
            <span class="line"></span>
          </div>
          <div class="bulletin">
            <p class="content">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="hideDetail">
        <i class="icon-close"></i>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import star from '../star/star';
export default {
  data() {
    return {
      detailShow: false
    };
  },
  props: {
    seller: {
      type: Object
    }
  },
  methods: {
    showDetail() {
      this.detailShow = true;
    },
    hideDetail() {
      this.detailShow = false;
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  components: {
    star
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin";
  .header
    position: relative
    overflow: hidden
    color #fff
    background rgba(7,17,27,0.5)
    .header-top
      position: relative
      padding: 24px 12px 18px 24px
      .avatar
        display inline-block
        img
        border-radius 2px
      .content
        display inline-block
        vertical-align top
        margin-left 16px
        .title
          margin 2px 0 8px 0
          .brand
            bg-image('brand')
            width: 30px;
            height: 18px;
            vertical-align top
            background-size 30px 18px
            display inline-block
          .name
            font-size 16px
            color rgb(255,255,255)
            font-weight bold
            line-height 18px
        .description
          margin-bottom 10px
          font-size 12px
          font-weight 200
          line-height 12px
        .support
          .icon
            width 12px
            margin-right 4px
            display inline-block
            height 12px
            background-size 12px 12px
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
          .text
            font-size 10px
            font-weight 200
            line-height: 12px;
        .support-count
          position: absolute
          right: 12px
          bottom: 14px
          padding: 0 8px
          height: 24px
          line-height: 24px
          border-radius: 14px
          background: rgba(0, 0, 0, 0.2)
          text-align: center
          .count
            vertical-align: top
            font-size: 10px
          .icon-keyboard_arrow_right
            margin-left: 2px
            line-height: 24px
            font-size: 10px
    .header-bottom
      position relative
      height 28px
      line-height 28px
      padding 0 22px 0 12px
      white-space nowrap
      overflow hidden
      text-overflow ellipsis
      background rgba(7,17,27,0.2)
      .notice
        line-height 28px
        display inline-block
        width 22px
        height: 12px;
        background-size 22px 12px
        bg-image('bulletin')
      .notice-text
        font-size 10px
        margin  0 4px
        line-height 28px
        font-weight 200
      .icon-keyboard_arrow_right
        position absolute
        right 12px
        top 8px
        font-size: 10px
    .background
      position absolute
      top:0
      left:0
      width 100%
      height 100%
      z-index -1
      filter: blur(10px)
    .detail
      position fixed
      z-index 100
      top 0
      left 0
      width 100%
      height 100%
      overflow auto
      opacity 1
      background: rgba(7,17,27,0.8)
      .detail-wrapper
        width: 100%
        min-height 100%
        .detail-main
          margin 64px 36px 0 36px
          .name
            font-size 16px
            font-weight 700
            color rgb(255,255,255)
            line-height 16px
            text-align center
          .star-wrapper
            margin-top 16px
            paddiing: 2px 0
            text-align center
          .title
            margin 28px 0 24px
            display flex
            .line
              flex 1
              position relative
              top -6px
              border-bottom: 1px solid rgba(255,255,255,0.2)
            .text
              font-size 14px
              font-weight 700
              line-height 14px
              margin 0 12px
          .supports-wrapper    
            .supports
              .support-item
                padding 0 12px
                margin-bottom 12px
                &:last-child
                  margin-bottom 0
                .icon
                  display inline-block
                  width 16px
                  height 16px
                  background-size 16px 16px
                  vertical-align top
                  margin-right 6px
                  &.decrease
                    bg-image('decrease_2')  
                  &.discount
                    bg-image('discount_2')  
                  &.guarantee
                    bg-image('guarantee_2')  
                  &.invoice
                    bg-image('invoice_2')
                  &.special
                    bg-image('special_2') 
                .text
                  font-size 12px
                  font-weight 200
                  line-height 12px     
          .bulletin
            .content
              font-size 12px
              font-weight 200
              line-height 24px
              padding: 0 12px
      .detail-close
        position relative
        font-size 32px
        width 32px
        height 32px 
        bottom 32px
        margin -32px auto 0
        color rgba(255,255,255,0.5)
</style>
