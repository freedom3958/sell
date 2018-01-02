<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <div class="overview">
        <h2 class="title">{{seller.name}}</h2>
        <div class="desc">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2 class="title">起送价</h2>
            <span class="price">
              <span class="num">{{seller.minPrice}}</span>元
            </span>
          </li>
          <li class="block">
            <h2 class="title">商家配送</h2>
            <span class="price">
              <span class="num">{{seller.deliveryPrice}}</span>元
            </span>
          </li>
          <li class="block">
            <h2 class="title">平均配送时间</h2>
            <span class="price">
              <span class="num">{{seller.deliveryTime}}</span>分钟
            </span>
          </li>
        </ul>
        <div class="favorite" @click="toggleFavorite">
          <span class="icon-favorite" :class="{'active':favorite}"></span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h2 class="title">公告与活动</h2>
        <div class="content-wrapper">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul class="supports">
          <li v-for="item in seller.supports" class="support-item">
            <span class="icon" :class="classMap[item.type]"></span>
            <span class="text">{{item.description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h2 class="title">商家实景</h2>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-list" ref="picList">
            <li class="pic-item" v-for="pic in seller.pics">
              <img :src="pic" width="120" height="90">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="info">
        <h2 class="title">商家信息</h2>
        <ul>
          <li class="info-item" v-for="item in seller.infos">
            {{item}}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import {saveToLocal, loadFromLocal} from '../../common/js/store';
  import star from '../star/star.vue';
  import split from '../split/split';
  import BScroll from 'better-scroll';

  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        favorite: (() => {
          return loadFromLocal(this.seller.id, 'favorite', false);
        })()
      };
    },
    mounted() {
      this.$nextTick(() => {
        this._initScroll();
        this._initPics();
      });
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    methods: {
      toggleFavorite() {
        this.favorite = !this.favorite;
        console.log(this.seller.id);
        saveToLocal(this.seller.id, 'favorite', this.favorite);
      },
      _initScroll() {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.seller, {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      },
      _initPics() {
        if (this.seller.pics) {
          let picWidth = 120;
          let margin = 6;
          let width = (picWidth + margin) * this.seller.pics.length - margin;
          this.$refs.picList.style.width = width + 'px';
          console.log(width);
          this.$nextTick(() => {
            this.picScroll = new BScroll(this.$refs.picWrapper, {
              scrollX: true
            });
          });
        }
      }
    },
    computed: {
      favoriteText() {
        if (this.favorite) {
          return '已收藏';
        } else {
          return '收藏';
        }
      }
    },
    components: {
      star,
      split
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl"

.seller
  position absolute
  left 0
  top 174px
  bottom 0
  width: 100%
  overflow hidden
  .overview
    position relative
    padding 18px
    .title
      font-size 14px
      color rgb(7,17,27)
      line-height 14px
      margin-bottom 8px
    .desc
      padding-bottom 18px
      border-bottom 1px solid rgba(7,17,27,0.1)
      .star
        display inline-block
        vertical-align top
        margin-right 8px
      .text
        display: inline-block
        vertical-align top
        margin-right 12px
        font-size 10px
        color rgb(77,85,93)
        line-height 18px
    .remark
      padding-top 18px
      display flex
      .block
        flex 1
        text-align center
        border-right 1px solid rgba(7,17,27,0.1)
        &:last-child
          border none
        .title
          font-size 10px
          color rgb(147,153,159)  
          line-height 10px
        .price
          font-size 10px
          font-weight 200
          color rgb(7,17,27)
          .num
            font-size 24px
            line-height 24px
    .favorite
      position absolute
      width 50px
      right 11px
      top 18px
      text-align center
      .icon-favorite
        display block
        margin-bottom 4px
        line-height 24px
        font-size 24px
        color #d4d6d9
        &.active
          color rgb(240,20,20)
      .text
        line-height 10px
        font-size 10px
        color rgb(77,85,93)    
  .bulletin
    padding 18px 18px 0 18px
    .title
      font-size 14px
      color rgb(7,17,27)
      line-height 14px
      margin-bottom 8px
    .content-wrapper
      padding  0 12px 16px 12px
      .content
        font-size 12px
        font-weight 200
        color rgb(240,20,20)
        line-height 24px
    .supports
      .support-item
        padding 16px 12px
        border-top 1px solid rgba(7,17,27,0.1)
        .icon
          width 16px
          height 16px
          display inline-block
          margin-right 6px
          background-size 16px 16px
          &.decrease
            bg-image('decrease_4')
          &.discount
            bg-image('discount_4')
          &.guarantee
            bg-image('guarantee_4')
          &.invoice
            bg-image('invoice_4')
          &.special
            bg-image('special_4')
        .text
          font-size 12px
          font-weight 200
          color rgb(7,17,27)      
          line-height 16px
  .pics
    padding 18px
    .title
      font-size 14px
      color rgb(7,17,27)
      line-height 14px
      margin-bottom 12px
    .pic-wrapper
      overflow hidden
      width 100%
      white-space nowrap
      .pic-list
        font-size 0
        .pic-item
          display inline-block
          margin-right 6px
          width 120px
          height 90px
          &:lasht-child
            margin 0
  .info
    padding 18px
    .title
      font-size 14px
      color rgb(7,17,27)
      line-height 14px
      margin-bottom 12px
    .info-item
      border-top 1px solid rgba(7,17,27,0.1)
      padding 16px 12px
      font-size 12px
      font-weight 200
      color rgb(7,17,27) 
      line-height 16p
</style>