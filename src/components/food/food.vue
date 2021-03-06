<template>
  <transition name="move">
    <div class="food" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="hideFood">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <div class="title">{{food.name}}</div>
          <div class="detail">
            <span class="month-sell">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="new">￥{{food.price}}</span>
            <span class="old" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper" v-if="food.count>0">
            <cartcontrol :food="food"></cartcontrol>
          </div>
          <transition name="fade">
            <div class="buy" v-show="!food.count || food.count===0" @click.stop.prevent="buy">
              加入购物车
            </div>
          </transition>
        </div>
        <split></split>
        <div class="info" v-show="food.info">
          <h2 class="title">商品信息</h2>
          <p class="text">{{food.info}}</p>
        </div>
        <split v-show="food.info"></split>
        <div class="rating">
          <h2 class="title">商品评价</h2>
          <ratingselect @toggle="toggleRating" @select="selectRating" :selectType="selectType" :onlyContent="onlyContent" :ratings="food.ratings" :desc="desc"></ratingselect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-for="rating in food.ratings" class="rating-item" v-show="needShow(rating.rateType,rating.text)">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img :src="rating.avatar" width="12" height="12" class="avatar">
                </div>
                <div class="time-wrapper">
                  <timeSwitch :ratingTime="rating.rateTime"></timeSwitch>
                </div>
                <p class="text">
                  <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
                  {{rating.text}}
                </p>
              </li>
            </ul>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">
              暂无评价
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import split from '../split/split.vue';
  import ratingselect from '../ratingselect/ratingselect.vue';
  import BScroll from 'better-scroll';
  import Vue from 'vue';
  import timeSwitch from '../timeSwitch/timeSwitch.vue';

  const ALL = 2;
  export default{
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        onlyContent: true,
        selectType: ALL,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    methods: {
      hideFood() {
        this.$emit('listenToHideFood');
      },
      buy() {
        Vue.set(this.food, 'count', 1);
      },
      toggleRating() {
        this.onlyContent = !this.onlyContent;
      },
      needShow(type, text) {
        if (!text && this.onlyContent) {
          return false;
        }
        if (this.selectType === ALL) {
          return true;
        } else {
          return type === this.selectType;
        }
      },
      selectRating(num) {
        this.selectType = num;
      },
      moveScroll() {
        this.$nextTick(() => {
          console.log(this.$refs.food.clientHeight);
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      }
    },
    components: {
      cartcontrol,
      split,
      ratingselect,
      timeSwitch
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .food
    z-index: 49
    position fixed
    left 0
    top 0
    width 100%
    bottom 48px
    background #fff
    padding-bottom 500px
    .image-header
      position relative
      left 0
      top 0
      width 100%
      height 0
      padding-top 100%
      img
        position absolute
        top 0
        left 0
        width 100%
        height 100%
      .back
        position absolute
        left 0
        top 0
        .icon-arrow_lift
          display block
          padding 10px
          font-size 20px
          color #fff
    .content
      position relative
      padding 18px
      .title
        font-size 14px
        line-height 14px
        margin-bottom 8px
        font-weight 700
        color rgb(7,17,27)
      .detail
        margin-bottom 18px
        linhe-height 10px
        height 10px
        font-size 0
        .month-sell,.rating
          font-size 10px
          color rgb(147,153,159)
          line-height 10px
        .month-sell
          margin-right 12px
      .price
        font-weight 700
        line-height 24px
        .new
          margin-right 8px
          font-size 14px
          color rgb(240,20,20)
        .old
          text-decoration line-through
          font-size 10px
          color rgb(147,153,159)
      .cartcontrol-wrapper
        position absolute
        right 12px
        bottom 12px
      .buy
        position absolute
        right 18px
        bottom 18px
        height 24px
        line-height 24px
        padding 0 12px
        color #fff
        background rgb(0,160,220)
        box-sizing border-box
        border-radius 12px
        font-size 10px
    .info
      padding 18px
      .title
        line-height 14px
        margin-bottom 6px
        font-size 14px
        color rgb(7,17,27)
        font-weight 530
      .text
        line-height 24px
        font-size 12px
        font-weight 200
        padding 0 8px
        color rgb(77,85,93)
    .rating
      margin-top  18px
      .title
        line-height 14px
        margin-left 18px
        font-size 14px
        color rgb(7,17,27)
        margin-bottom 6px
      .rating-wrapper
        padding 0 18px
        .rating-item
          position relative
          padding 16px 0
          border-bottom 1px solid rgba(147,153,159,0.1)
          .user
            position absolute
            right 0
            line-height 12px
            top 16px
            font-size 0
            .name
              display inline-block
              vertical-align top
              font-size 10px
              color rgb(147,153,159)
              margin-right 6px
            .avatar
              border-radius 50%
          .time-wrapper
            margin-bottom 6px
            line-height 12px
            font-size 10px
            color rgb(147,153,159)
          .text
            line-height 16px
            color rgb(7,17,27)
            font-size 12px
            .icon-thumb_up,.icon-thumb_down
              margin-right 4px
              font-size 12px
              line-height: 16px
            .icon-thumb_up
              color rgb(0,160,220)
            .icon-thumb_down
              color rgb(147,153,159)
        .no-rating
          padding 16px 0
          font-size 12px
          color rgb(147,153,159)
</style>

