<template>
	<div class="star" :class="starType">
		<span v-for="(itemClass,index) in itemClasses" :class="itemClass" class="star-item" :key="index"></span>
	</div>
</template>

<script type="text/ecmascript-6">
	const LENGTH = 5;
	const CLS_ON = 'ON';
	const CLS_OFF = 'OFF';
	const CLS_HALF = 'HALF';
	export default{
		props: {
			size: {
				type: Number
			},
			score: {
				type: Number
			}
		},
		computed: {
			starType() {
				return 'star-' + this.size;
			},
			itemClasses() {
				let result = [];
				let score = Math.floor(this.score * 2) / 2;
				let hasDecimal = score % 1 !== 0;
				let int = Math.floor(score);
				for (let item = 0; item < int; item++) {
					result.push(CLS_ON);
				}
				if (hasDecimal) {
					result.push(CLS_HALF);
				}
				while (result.length < LENGTH) {
					result.push(CLS_OFF);
				}
				return result;
			}
		}
	};
</script>

<style lang="stylus" rel="stylesheet/stylus">
	@import "../../common/stylus/mixin.styl"

	.star
		font-size 0
		.star-item
			display: inline-block
			background-repeat: no-repeat
		&.star-48
			.star-item
				width: 20px
				height: 20px
				margin-right: 22px		
				background-size: 20px 20px
				&:last-child
					margin:0
				&.ON
					bg-image('star48_on')
				&.HALF
					bg-image('star48_half')
				&.OFF
					bg-image('star48_off')
		&.star-36
			.star-item
				width: 15px
				height: 15px
				margin-right: 6px
				background-size: 15px 15px
				&:last-child
					margin:0
				&.ON
					bg-image('star36_on')
				&.HALF
					bg-image('star36_half')
				&.OFF
					bg-image('star36_off')
		&.star-24
			.star-item
				width: 10px
				height: 10px
				margin-right: 3px
				background-size: 10px 10px
				&:last-child
					margin:0
				&.ON
					bg-image('star24_on')
				&.HALF
					bg-image('star24_half')
				&.OFF
					bg-image('star24_off')
</style>
