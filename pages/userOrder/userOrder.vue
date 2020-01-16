<template>
	<view>
		<view class="orderNav flex fs14 whiteBj color6">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">待支付</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">待接单</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">服务中</view>
			<view class="tt" :class="current==5?'on':''" @click="change('5')">已完成</view>
		</view>
		<view class="pdtb20"></view>
		<view class="pdlr4 mgt15">
			<view class="proRow">
				<view class="item">
					<view class="flexRowBetween fs12 mgb10">
						<view class="color9">交易时间：2018-08-30</view>
						<view class="red">待支付</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/the-order-img1.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow2">马桶疏通</view>
							<view class="B-price">
								<view class="price fs16 ftw">138</view>
							</view>
						</view>
					</view>
					<div class="underBtn flexEnd">
						<span class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/orderConfirm/orderConfirm'}})">去支付</span>
					</div>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/userOrderDetail/userOrderDetail'}})">
					<view class="flexRowBetween fs12 mgb10">
						<view class="color9">交易时间：2018-08-30</view>
						<view class="red">待接单</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/the-order-img1.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow2">马桶疏通</view>
							<view class="B-price">
								<view class="price fs16 ftw">138</view>
							</view>
						</view>
					</view>
				</view>
				
				<view class="item">
					<view  @click="Router.navigateTo({route:{path:'/pages/userOrderDetail/userOrderDetail'}})">
						<view class="flexRowBetween fs12 mgb10">
							<view class="color9">交易时间：2018-08-30</view>
							<view class="red">服务中</view>
						</view>
						<view class="flexRowBetween">
							<view class="pic">
								<image src="../../static/images/the-order-img1.png" mode=""></image>
							</view>
							<view class="infor">
								<view class="title avoidOverflow2">马桶疏通</view>
								<view class="B-price">
									<view class="price fs16 ftw">138</view>
								</view>
							</view>
						</view>
					</view>
					<div class="underBtn flexEnd" @click="alertShow">
						<span class="Bbtn">确认完成</span>
					</div>
				</view>
			</view>
		</view>
		
		<!-- 确认完成弹框 -->
		<view class="black-bj" v-show="is_show"></view>
		<view class="alertShow center whiteBj radius10 pdt20" v-show="is_alertShow">
			<view class="tip-title mgb10 fs15">提示</view>
			<view class="fs13 color6 pdb20 borderB1">该服务确定已为您完成服务？</view>
			<view class="flex tip-button">
				<view class="item"  @click="alertShow">取消</view>
				<view class="item pubColor">确定</view>
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
				is_show:false,
				current:1,
				is_alertShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
				}
			},
			alertShow(){
				const self=this;
				self.is_alertShow = !self.is_alertShow;
				self.is_show = !self.is_show;
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/myOrder.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	
</style>

