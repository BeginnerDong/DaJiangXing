<template>
	<view>
		
		<view class="detailxqBan">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		<view class="mglr4 pdtb15 fs15">
			<view class="tit mgb10">{{mainData.title}}</view>
			<view class="ftw pubColor"><span class="ftn fs14">￥</span>{{mainData.price}}</view>
		</view>
		<view class="f5H10"></view>
		<view class="mglr4 pdtb15">
			<view class="ftw fs15 pdb10">详情介绍</view>
			<view class="xqInfor fs13">
				<view class="cont">
					<view class="content ql-editor" style="padding:0;"
					v-html="mainData.content">
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar pdlr4">
			<view class="ite" @click="tel()">
				<view><image src="../../static/images/details-icon.png" mode=""></image></view>
				<view  class="mgt5">电话客服</view>
			</view>
			<view class="submitbtn" style="width: 70%;" @click="Utils.stopMultiClick(goBuy)">
				<button class="Wbtn" type="button" >立即预约</button>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				Utils:this.$Utils,
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},
		
		methods: {
			
			goBuy(){
				const self = this;
				uni.setStorageSync('canClick',false);
				self.orderList.push(
					{product_id:self.mainData.id,count:1,
					type:1,product:self.mainData},
				);
				uni.setStorageSync('payPro', self.orderList);
				
				self.Router.navigateTo({route:{path:'/pages/orderConfirm/orderConfirm'}})
				
				uni.setStorageSync('canClick',true);
			},
			
			//拨打客服电话
			tel(){
				const self = this;
				uni.makePhoneCall({
					phoneNumber: self.mainData.phone //仅为示例
				});
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	page{padding-bottom: 150rpx;}
</style>
