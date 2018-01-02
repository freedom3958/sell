<template>
  <div class="ratings" ref="ratings">
    <div class="ratinigs-content">
      <div class="overview">
        <div class="overview-left">
          <h2 class="score">{{seller.score}}</h2>
          <div class="title">综合评分</div>
          <div class="rank">高于周边卖家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :score="seller.serviceScore" :size="36"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :score="seller.foodScore" :size="36"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingselect @toggle="toggleRating" @select="selectRating" :selectType="selectType" :onlyContent="onlyContent" :ratings="ratings"></ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li v-for="rating in ratings" class="rating-item" v-show="needShow(rating.rateType, rating.text)">
            <div class="avatar">
              <img :src="rating.avatar" width="28" height="28">
            </div>
            <div class="content">
              <h2 class="name">{{rating.username}}</h2>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery">
                {{rating.deliveryTime}}分钟送达
                </span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend">
                <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
                <span class="item" v-for="item in rating.recommend">{{item}}</span>
              </div>
              <div class="time-wrapper">
                <timeSwitch :ratingTime="rating.rateTime"></timeSwitch>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import star from '../star/star.vue';
  import split from '../split/split.vue';
  import ratingselect from '../ratingselect/ratingselect.vue';
  import timeSwitch from '../timeSwitch/timeSwitch.vue';

  const ERR_OK = 0;
  const ALL = 2;
  export default{
    data() {
      return {
        ratings: [],
        onlyContent: true,
        selectType: ALL
      };
    },
    props: {
      seller: {
        type: Object
      }
    },
    created() {
      this.$http.get('/api/ratings').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.ratings = response.data;
          this.$nextTick(() => {
            this.scroll = new BScroll(this.$refs.ratings, {
              click: true
            });
          });
        }
      });
    },
    methods: {
      toggleRating() {
        this.onlyContent = !this.onlyContent;
      },
      selectRating(type) {
        this.selectType = type;
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
      }
    },
    components: {
      star,
      split,
      ratingselect,
      timeSwitch
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .ratings
    position absolute
    top 174px
    bottom 0
    left 0
    width 100%
    overflow hidden
    .overview
      display flex
      padding 18px 0
      .overview-left
        flex 0 0 137px
        border-right 1px solid rgba(7,17,27,0.1)
        width 137px
        text-align center
        @media only screen and (max-width: 320px)
          flex 0 0 120px
          width 120px
        .score
          font-size 24px
          color rgb(255,153,0)
          line-height 28px
          margin-bottom 6px
        .title
          font-size 12px
          margin-bottom 8px
          corlor rgb(7,17,27)
          line-height 12px
        .rank
          font-size 10px
          color rgb(147,153,159)
          line-height 10px
      .overview-right
        flex 1
        padding-left 24px
        @media only screen and (max-width: 320px)
          padding-left 6px
        .score-wrapper
          margin-bottom 8px
          .title
            display inline-block
            line-height 18px
            color rgb(7,17,27)
            font-size 12px
          .star  
            display inline-block
            margin 0 8px
          .score
            display inline-block
            line-height 18px
            font-size 12px
            color rgb(147,153,159)
        .delivery-wrapper
          .title
            font-size 12px
            color rgb(7,17,27)
            line-height 18px
          .delivery
            margin-left 12px
            sont-size 12px
            linhe-height 18px
            color rgb(147,153,159)
    .rating-wrapper
      padding 0 18px
      .rating-item
        display flex
        padding 18px 0
        border-bottom 1px solid rgba(7,17,27,0.1)
        .avatar
          flex 0 0 28px
          width 28px
          margin-right 12px
          img
            border-radius 50%
        .content
          flex 1
          position relative
          .name
            margin-bottom 4px
            line-height 12px
            font-size 10px
            color rgb(7,17,27)
          .star-wrapper
            margin-bottom 6px
            font-size 0
            .star  
              margin-right 6px
              display inline-block
            .delivery
              display inline-block
              line-height 12px
              font-size 10px
              font-weight 200
              color rgb(147,153,159)
          .text
            margin-bottom 8px
            line-height 18px
            color rgb(7,17,27)
            font-size 12px
          .recommend
            line-height 16px
            .icon-thumb_up
              font-size 12px
              color rgb(0,160,220)
              line-height 16px
              display inline-block
              margin-right 8px
            .icon-thumb_down
              font-size 12px
              color rgb(183,187,191)
              line-height 16px
              display inline-block
              margin-right 8px
            .item
              padding 0 6px
              margin-right 8px  
              font-size 9px
              color rgb(147,153,159)
              line-height 16px
              border 1px solid rgba(7,17,27,0.1)
              border-radius 1px
              background #fff
          .time-wrapper
            position absolute
            right 18px
            top 0
            line-height 12px
            color rgb(147,153,159)
            font-weight 200
            font-size 10px    
</style>
