<template>
	<view>
		
		<view class="contactUs pdlr4">
			<view class="item flexRowBetween">
				<view class="icon"><image src="../../static/images/contact-us-icon.png" mode=""></image></view>
				<view class="rr">15623562356</view>
			</view>
			<view class="item flexRowBetween">
				<view class="icon"><image src="../../static/images/contact-us-icon1.png" mode=""></image></view>
				<view class="rr">陕西省西安市雁塔区高新大都荟</view>
			</view>
			<view class="item flexRowBetween">
				<view class="icon"><image src="../../static/images/contact-us-icon2.png" mode=""></image></view>
				<view class="rr flex">
					<view>15623562356</view>
					<view><image class="ewm" src="../../static/images/contact-us-img.png" mode=""></image></view>
				</view>
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	.contactUs .item{padding-top: 30rpx;}
	.contactUs .icon{width: 60rpx;height: 60rpx;}
	.contactUs .icon image{width: 100%;height: 100%;display: block;}
	.contactUs .item .rr{width: 87%;}
	.contactUs .item .ewm{margin-left: 30rpx;width: 120rpx;height: 120rpx;}
</style>
