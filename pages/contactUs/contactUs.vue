<template>
	<view>
		
		<view class="contactUs pdlr4">
			<view class="item flexRowBetween" @click="tel">
				<view class="icon"><image src="../../static/images/contact-us-icon.png" mode=""></image></view>
				<view class="rr">{{mainData.description}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="icon"><image src="../../static/images/contact-us-icon1.png" mode=""></image></view>
				<view class="rr">{{mainData.url}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="icon"><image src="../../static/images/contact-us-icon2.png" mode=""></image></view>
				<view class="rr flex">
					<view>15623562356</view>
					<view @click="previewImage(0)"><image class="ewm" :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image></view>
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
				mainData:{}
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			previewImage(index) {
				console.log(1);
				const self = this;
				var urls = [];
				for (var i = 0; i < self.mainData.mainImg.length; i++) {
					urls.push(self.mainData.mainImg[i].url)
				};
				var current = self.mainData.mainImg[index].url; //这里获取到的是一张本地的图片
				wx.previewImage({
					current: current, //需要预览的图片链接列表
					urls: urls //当前显示图片的链接
				})
			},
			
			tel(){
				const self = this;
				uni.makePhoneCall({
					phoneNumber: self.mainData.description //仅为示例
				});
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					title:'联系我们',
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.labelGet(postData, callback);
			},
		}
	};
</script>

<style>
	.contactUs .item{padding-top: 30rpx;}
	.contactUs .icon{width: 60rpx;height: 60rpx;}
	.contactUs .icon image{width: 100%;height: 100%;display: block;}
	.contactUs .item .rr{width: 87%;}
	.contactUs .item .ewm{margin-left: 30rpx;width: 120rpx;height: 120rpx;}
</style>
