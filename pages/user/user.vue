<template>
	<view>
		<view class="center white" style="font-size: 16px;line-height: 44px;height: 44px;background: #FF5C11;">我的</view>
		
		<view class="userHead white pdlr4">
			<view class="infor">
				<view class="left flex">
					<image class="photo" src="../../static/images/about-img.png" mode=""></image>
					<view style="width: 70%;">
						<view class="fs14 pdb5">昵称昵称昵称昵称</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="">	
			<view class="userBox2 boxShaow radius10 whiteBj">
				<view class="tit flexRowBetween pdlr4 pdt15 pdb10 borderB1">
					<view>我的订单</view>
					<view class="more flexEnd fs12 color9" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">全部订单<image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image></view>
				</view>
				<view class="menu flexRowBetween color6 fs12 pdtb15 center">
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<image src="../../static/images/about-icon1.png"></image>
						<view>待支付</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<image src="../../static/images/about-icon2.png"></image>
						<view>待发货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})">
						<image src="../../static/images/about-icon3.png"></image>
						<view>待收货</view>
					</view>
					<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrder/userOrder'}})" >
						<image src="../../static/images/about-icon4.png"></image>
						<view>已完成</view>
					</view>
				</view>
			</view>
			
			<view class="myRowBetween fs13 mglr4 mgt15">
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})" >
					<view class="ll flex">
						<image class="icon" src="../../static/images/about-icon4.png" mode=""></image>
						<view class="">我的地址</view>
					</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/contactUs/contactUs'}})" >
					<view class="ll flex">
						<image class="icon" src="../../static/images/about-icon5.png" mode=""></image>
						<view class="">联系我们</view>
					</view>
					<view class="rr"><image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image></view>
				</view>
			</view>
		</view>
			
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	.userHead{height:280rpx;background:#FF5C11;box-sizing: border-box;padding-top: 40rpx;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;border: 2px solid #ff8851;box-sizing: border-box;}
	
	.userBox2{margin: -90rpx 4% 10rpx 4% ;position: relative; z-index: 1; box-shadow: 0 10rpx 20rpx rgba(0,0,0,0.1);}
	.userBox2 .item{width: 25%;}
	.userBox2 .item image{width: 48rpx;height:48rpx;display: block;margin: 0 auto;margin-bottom: 10rpx;}
	
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
</style>
