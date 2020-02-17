<template>
	<view>
		<view class="mglr4 pdtb10">
			<view class="whiteBj radius10 mgb15">
				<view class="flex">
					<image class="w" style="" src="../../static/images/the-order-img.png" mode="widthFix"></image>
				</view>
				<view class="flexRowBetween pdtb15 mglr4">
					<view class="fs13">
						<view>{{addressData.city+addressData.detail}}</view>
						<view class="color6 flex mgt5">
							<view class="mgr10">{{addressData.name}}</view>
							<view class="">{{addressData.phone}}</view>
						</view>
					</view>
					<!-- <view class="flexEnd" style="width: 20%;">
						<image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image>
					</view> -->
				</view>

			</view>

			<view class="proRow radius10 whiteBj mgb15">
				<view class="item flexRowBetween">
					<view class="pic">
						<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
							mainData.orderItem[0].snap_product.mainImg&&mainData.orderItem[0].snap_product.mainImg[0]?mainData.orderItem[0].snap_product.mainImg[0].url:''"
						 mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow fs13 mgt5">{{mainData.title}}</view>
						<view class="flexRowBetween B-price">
							<view class="price fs14">{{mainData.price}}</view>
							<view class="flexEnd">
								<view class=" flex">

									<view class="">x{{mainData.count}}</view>

								</view>
							</view>
						</view>
					</view>
				</view>
			</view>

			<view class="whiteBj radius10">
				<view class="myRowBetween pdlr4">
					<view class="item flexRowBetween">
						<view class="ll fs13">微信号</view>
						<view class="rr fs12">
							<input type="text" v-model="wechat" disabled="true" placeholder="请输入微信号,以便客服联系" placeholder-class="placeholder" />
						</view>
					</view>
				</view>
			</view>

		</view>

		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween" style="height: 100rpx;">
			<view class="left flex mgl15">
				<view class="fs16 price">{{mainData.price}}</view>
			</view>
			<button class="payBtn fs16 white" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(goPay)">立即支付</button>
		</view>
		<!-- 底部菜单按钮 end -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils: this.$Utils,

				wechat: '',
				mainData: {},
				addressData: {},
				totalPrice: 0,
				pay: {},
				id:''
			}
		},

		onLoad(options) {
			const self = this;
			
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self)
		},



		methods: {


			getMainData() {
				const self = this;
				console.log(23435345)
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id: self.id
				};
				postData.getAfter = {
					orderItem: {
						tableName: 'OrderItem',
						middleKey: 'order_no',
						key: 'order_no',
						searchItem: {
							status: 1
						},
						condition: '='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.addressData = self.mainData.snap_address;
						self.wechat = self.mainData.wechat;
						self.pay.wxPay = {
							price: self.mainData.price,
						};
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},



			/* countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += self.mainData[i].product.price * self.mainData[i].count
				};
				var wxPay = parseFloat(self.totalPrice).toFixed(2)
				//console.log('wxPay',wxPay)
				if (wxPay > 0) {
					self.pay.wxPay = {
						price: wxPay,
					};
				} else {
					delete self.pay.wxPay;
				};
				console.log(self.pay)
			}, */



			goPay(order_id) {
				const self = this;
				uni.setStorageSync('canClick', false);

				const postData = self.$Utils.cloneForm(self.pay)
				postData.tokenFuncName = 'getProjectToken',
					postData.searchItem = {
						id: self.id
					};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {

										}
									});
									setTimeout(function() {

										uni.navigateBack({
											delta: 1
										})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {

							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {

								}
							});
							setTimeout(function() {
								uni.navigateBack({
									delta: 1
								})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/detail.css";

	page {
		background: #F5F5F5;
		padding-bottom: 120rpx;
	}

	button {
		background: none;
		line-height: 1.5;
		margin-left: 0;
		margin-right: 0;
		font-size: 16px;
		border-radius: 0;
	}

	button::after {
		border: none;
	}

	.proRow .item {
		padding: 30rpx 4%;
	}

	.proRow .item .pic {
		width: 190rpx;
		height: 190rpx;
	}

	.proRow .item .pic image {
		width: 100%;
		height: 100%;
		display: block;
	}

	.proRow .item .infor {
		width: 66%;
		height: 190rpx;
		position: relative;
	}

	.proRow .item .B-price {
		position: absolute;
		left: 0;
		right: 0;
		bottom: 10rpx;
	}

	.myRowBetween .item:last-child {
		border-bottom: 0;
	}

	/* 数量加减 */
	.numBox {
		border: 1px solid #eee;
		border-radius: 8rpx;
	}

	.numBox view {
		width: 44rpx;
		height: 44rpx;
		color: #999;
		font-size: 32rpx;
		text-align: center;
		line-height: 44rpx;
	}

	.numBox view.num {
		width: 60rpx;
		color: #333;
		font-size: 26rpx;
		border-left: 1px solid #eee;
		border-right: 1px solid #eee;
	}
</style>
