<template>
	<view>
		
		<view class="detailxqBan">
			<image src="../../static/images/details-img.png" mode=""></image>
		</view>
		<view class="mglr4 pdtb15 fs15">
			<view class="tit mgb10">电路检修</view>
			<view class="ftw pubColor"><span class="ftn fs14">￥</span>135</view>
		</view>
		<view class="f5H10"></view>
		<view class="mglr4 pdtb15">
			<view class="ftw fs15 pdb10">详情介绍</view>
			<view class="xqInfor fs13">
				<view class="cont">
					<view>管理客服电话还房贷两个号和个梵蒂冈悲愤交加鹤骨鸡肤供货方点击可供货方都拉黑干活的放假开个会过分的话看</view>
					<view>花港饭店两三个号换个卡富家大室联合国利干活附近的开个会发和公交卡方大化工干活附近的看和</view>
					<view><image class="w" src="../../static/images/details-img1.png" mode="widthFix"></image></view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar pdlr4">
			<view class="ite" @click="tel()">
				<view><image src="../../static/images/details-icon.png" mode=""></image></view>
				<view  class="mgt5">电话客服</view>
			</view>
			<view class="submitbtn" style="width: 70%;" @click="Router.navigateTo({route:{path:'/pages/orderConfirm/orderConfirm'}})">
				<button class="Wbtn" type="button">立即预约</button>
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
				wx_info:{},
				is_show:false
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			//拨打客服电话
			tel(){
				uni.makePhoneCall({
					phoneNumber: '18932145667' //仅为示例
				});
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	page{padding-bottom: 150rpx;}
</style>
