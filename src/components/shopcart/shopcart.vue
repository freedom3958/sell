<template>
  <div class="shopcart">
    <div class="content" @click="toggleList">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}">
            <i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalCount>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" @click.stop.prevent="pay">
        <div class="pay" :class="payClass">
          {{payDesc}}
        </div>
      </div>
    </div>
    <transition name="fold">
      <div class="shopcart-list" v-show="listShow">
        <div class="list-header">
          <h3 class="title">购物车</h3>
          <span class="empty" @click="empty">清空</span>
        </div>
        <div class="list-content" ref="listContent">
          <ul>
            <li v-for="item in selectFoods" class="food-item">
              <span class="name">{{item.name}}</span>
              <div class="price">￥<span>{{item.price*item.count}}</span></div>
              <div class="cartcontrol-wrapper">
                <cartcontrol :food="item"></cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import BScroll from 'better-scroll';
  export default{
    props: {
      selectFoods: {
        type: Array,
        default() {
          return [
            {
              count: 0,
              price: 0
            }
          ];
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    data() {
      return {
        listShow: false
      };
    },
    methods: {
      empty() {
        this.selectFoods.forEach((food) => {
          food.count = 0;
        });
      },
      toggleList() {
        if (!this.totalCount) {
          return false;
        } else {
          this.listShow = !this.listShow;
        };
        this.listContent = new BScroll(this.$refs.listContent, {
          click: true
        });
      },
      pay() {
        if (this.totalPrice < this.minPrice) {
          return;
        }
        window.alert(`支付${this.totalPrice}元`);
      }
    },
    components: {
      cartcontrol
    },
    computed: {
      payClass() {
        if (this.totalPrice >= this.minPrice) {
          return 'enough';
        } else {
          return 'not-enough';
        }
      },
      payDesc() {
        if (this.totalPrice >= this.minPrice) {
          return '去结算';
        } else {
          let diff = this.minPrice - this.totalPrice;
          return `还差￥${diff}元起送`;
        }
      },
      totalCount() {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        if (count === 0) {
          this.listShow = false;
        }
        return count;
      },
      totalPrice() {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.count * food.price;
        });
        return total;
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
.shopcart
  position: fixed
  left: 0
  bottom: 0
  z-index: 50
  width: 100%
  height: 48px
  .content
    background #141d27
    display flex
    font-size: 0
    color rgba(255,255,255,0.4)
    .content-left
      flex:1
      .logo-wrapper
        background #141d27
        display inline-block
        vertical-align top
        position relative
        top -10px
        margin  0 12px
        padding 6px
        width 56px
        height 56px
        border-radius 50%
        box-sizing border-box
        .logo
          width 100%
          height 100%
          text-align center
          border-radius 50%
          background #2b343c
          &.highlight
            background rgb(0, 160, 220)
          .icon-shopping_cart
            line-height 44px
            font-size 24px
            color #80858a
            &.highlight
              color: #fff
        .num
          position absolute
          width 24px
          right 0
          tio 0
          height 16px
          line-height 16px
          font-size 9px
          font-weight 700
          border-radius 16px
          background rgb(240,20,20)
          text-align center
          box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)
          color #fff
          top 0
      .price
        display inline-block
        margin-top 12px
        vertical-align top
        font-size 16px
        font-weight 700
        line-height 24px
        padding-right 12px
        border-right 1px solid rgba(255,255,255,0.1)
        box-sizing border-box
      .desc
        display inline-block
        line-height 24px
        vertical-align top
        font-size 10px
        margin: 12px 0 0 12px
    .content-right
      flex 0 0 105px
      width 105px
      .pay
        height 48px
        line-height 48px
        text-align center
        font-size 12px
        &.enough
          background: #00b43c
          color: #fff
        &.not-enough
          background: #2b333b
  .shopcart-list
    position: absolute
    left: 0
    top 0
    z-index: -1
    width: 100%
    transform: translate3d(0,-100%,0)
    &.fold-enter-active,&fold
      transition all 0.5s
    &.fold-enter,&.fold-leave-active
      transform translate3d(0,0,0)
    .list-header
      height 40px
      background #f3f5f7
      border-bottom 1px solid rgba(7,17,27,0.1)
      .title
        float: left
        font-size 14px
        font-weight 200
        line-height 40px
        padding-left 18px
      .empty
        float right
        font-size 12px
        color rgb(0,160,220)
        line-height 40px
        padding-right 18px
    .list-content
      padding 0 18px
      overflow hidden
      max-height 217px
      background #fff
      .food-item
        position relative
        border-bottom 1px solid rgba(7,17,27,0.1)
        box-sizing border-box
        height 48px
        width 100%
        padding 12px 0
        .name
          float left
          font-size 14px
          color rgb(7,17,27)
          line-height 24px
        .price
          position absolute
          right 90px
          bottom 12px
          font-size 10px
          color rgb(240,20,20)
          font-weight normal
          line-height 24px
          span
            font-size 14px
            font-weight 700
        .cartcontrol-wrapper
          position: absolute
          right: 0
          bottom: 6px
</style>
