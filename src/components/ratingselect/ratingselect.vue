<template>
  <div class="ratingselect">
    <div class="rating-type">
      <span @click="select(2,$event)" class="block positive" :class="{'active':selectType===2}">{{desc.all}}
        <span class="count">{{ratings.length}}</span>
      </span>
      <span @click="select(0,$event)" class="block positive" :class="{'active':selectType===0}">{{desc.positive}}
        <span class="count">{{positives.length}}</span>
      </span>
      <span @click="select(1,$event)" class="block negative" :class="{'active':selectType===1}">{{desc.negative}}
        <span class="count">{{negatives.length}}</span>
      </span>
    </div>
    <div class="switch" @click="toggleContent" :class="{'on':onlyContent}">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
const ALL = 2;
const POSITIVE = 0;
const NEGATIVE = 1;

export default{
  props: {
    desc: {
      type: Object,
      default() {
        return {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        };
      }
    },
    onlyContent: {
      type: Boolean,
      default: false
    },
    ratings: {
      type: Array,
      default() {
        return [];
      }
    },
    selectType: {
      type: Number,
      default: ALL
    }
  },
  computed: {
    positives() {
      let positives = [];
      this.ratings.forEach((item) => {
        if (item.rateType === POSITIVE) {
          positives.push(item);
        }
      });
      return positives;
    },
    negatives() {
      let negatives = [];
      this.ratings.forEach((item) => {
        if (item.rateType === NEGATIVE) {
          negatives.push(item);
        }
      });
      return negatives;
    }
  },
  methods: {
    select(num, event) {
      this.$emit('select', num);
    },
    toggleContent() {
      this.$emit('toggle');
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .ratingselect
    .rating-type
      margin 12px 18px
      padding-bottom 18px
      border-bottom solid 1px rgba(7,17,27,0.1)
      .block
        display inline-block
        padding 8px 12px
        font-size 12px
        line-height 16px
        color rgb(77,85,93)
        &.active
          color #fff
        .count
          margin-left 2px
          font-size 8px
        &.positive
          background rgba(0,160,220,0.2)
          &.active
            background rgb(0,160,220)
        &.negative
          background rgba(77,85,93,0.2)
          &.active
            background rgb(77,83,93)
    .switch
      padding 12px 18px
      line-height 24px
      border-bottom 1px solid rgba(7,17,27,0.1)
      &.on
        .icon-check_circle
          color rgb(0,160,220)
      .icon-check_circle
        display inline-block
        margin-right 4px
        font-size 24px
        vertical-align top
      .text
        font-size 12px
        display inline-block
        color rgb(147,153,159)

</style>
