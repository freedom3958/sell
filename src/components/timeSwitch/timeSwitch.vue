<template>
  <div class="timeSwitch">{{ratingTime | formatDate}}</div>
</template>

<script type="text/ecmascript-6">
import {formatDate} from '../../common/js/date';

export default{
  props: {
    ratingTime: {
      type: Number
    }
  },
  filters: {
  	formatDate(time) {
  		let date = new Date(time);
  		return formatDate(date, 'yyyy-MM-dd hh:mm');
  	}
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">

</style>
