<template>
  <div>
    <div class="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <ul>
          <li v-for="(item,index) in goods" class="menu-item" @click="selectMenu(index,$event)" ref="menuList" :class="{'current':currentIndex===index}">
            <span class="text">
              <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
    			  </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref="foodsWrapper">
        <ul>
          <li v-for="item in goods" class="foods-list" ref="foodList">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li v-for="food in item.foods" class="food-item" @click="selectFood(food,$event)">
                <div class="icon">
                  <img :src="food.icon" width="57" height="57">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}</span><span>好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="new">￥{{food.price}}</span>
                    <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                  </div>
                  <div class="add-wrapper">
                    <cartcontrol :food="food" @add="addFood"></cartcontrol>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <shopcart ref="shopcart" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice" :selectFoods="selectFoods"></shopcart>
    </div>
    <food v-show="foodShow" ref="food" :food="selectedFood" @listenToHideFood="hideFood"></food>
  </div>
</template>

<script type="text/ecmascript-6">
  import shopcart from '../shopcart/shopcart';
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import BScroll from 'better-scroll';
  import food from '../food/food.vue';

  const ERR_OK = 0;
  export default{
  	props: {
  		seller: {
  			type: Object
  		}
  	},
  	data() {
  		return {
  			goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood: {},
        foodShow: false
  		};
  	},
  	created() {
  		this.$http.get('/api/goods').then((response) => {
  			response = response.body;
  			if (response.errno === ERR_OK) {
  				this.goods = response.data;
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          });
  			}
  		});
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  	},
    computed: {
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      },
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            this._followScroll(i);
            return i;
          }
        }
        return 0;
      }
    },
    methods: {
      addFood(target) {
        this._drop(target);
      },
      hideFood() {
        this.foodShow = false;
      },
      selectFood(food, event) {
        if (this._constructed) {
          return;
        }
        this.selectedFood = food;
        this.foodShow = true;
        console.log(this.$refs.food);
        this.$refs.food.moveScroll();
      },
      selectMenu(index, event) {
        let el = this.$refs.foodList[index];
        this.foodsScroll.scrollToElement(el, 300);
      },
      _drop(target) {
        this.$nextTick(() => {

        });
      },
      _initScroll() {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        });
        this.foodsScroll.on('scroll', (pos) => {
          if (pos.y <= 0) {
            this.scrollY = Math.abs(Math.round(pos.y));
          }
        });
      },
      _calculateHeight() {
        let foodList = this.$refs.foodList;
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          height += foodList[i].clientHeight;
          this.listHeight.push(height);
        }
      },
      _followScroll(index) {
        let menuList = this.$refs.menuList;
        let el = menuList[index];
        this.menuScroll.scrollToElement(el);
      }
    },
    components: {
      shopcart,
      cartcontrol,
      food
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px
      width: 80px
      background: #f3f5f7
      .menu-item
        display: table
        height: 54px
        width: 56px
        padding: 0 12px
        line-height: 14px
        &.current
          position: relative
          z-index: 10
          margin-top: -1px
          background: #fff
          font-weight: 700
          .text
            border none
        .icon
          display: inline-block
          vertical-align: top
          width: 12px
          height: 12px
          margin-right: 2px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')
        .text
          display: table-cell
          width: 56px
          vertical-align: middle
          font-size: 12px
          border-bottom 1px solid rgba(7,17,27,0.1)
    .foods-wrapper
      flex: 1
      .foods-list
        .title
          padding-left 18px
          font-size 12px
          color rgb(147,153,159)
          line-height 26px
          background #f3f5f7
        .food-item
          display flex
          margin 18px
          padding-bottom 18px
          border-bottom 1px solid rgba(7,17,27,0.1)
          .icon
            flex: 0 0 57px
            margin-right 10px
          .content
            position relative
            flex 1
            .name
              line-height 14px
              font-size 14px
              color rgb(7,17,27)
              height 14px
              margin 2px 0 8px
            .desc,.extra
              line-height 10px
              font-size 10px
              color rgb(147,153,159)
              margin-bottom 8px
            .extra
              margin-right 12px
              .count
                margin-right 12px
            .price
              font-weight normal
              line-height 24px
              .new
                font-weight 700
                margin-right 8px
                font-size 14px
                color rgb(240,20,20)
              .old
                color rgb(147,153,159)
                font-size 10px
                font-weight 700
                text-decoration line-through
            .add-wrapper
              position absolute
              right 0px
              bottom -6px

</style>
