<template>
	<view>
		<view class="mglr4 pdtb10">
			<view class="whiteBj radius10 mgb15">
				<view class="flex">
					<image class="w" style="" src="../../static/images/the-order-img.png" mode="widthFix"></image>
				</view>
				<view class="flexRowBetween pdtb15 mglr4" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<view class="fs13"  v-if="addressData.name">
						<view>{{addressData.city+addressData.detail}}</view>
						<view class="color6 flex mgt5">
							<view class="mgr10">{{addressData.name}}</view>
							<view class="">{{addressData.phone}}</view>
						</view>
					</view>
					<view class="fs13" v-else>
						<view>请选择收货地址</view>
					</view>
					<view class="flexEnd" style="width: 20%;">
						<image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image>
					</view>
				</view>
				
			</view>

			<view class="proRow radius10 whiteBj mgb15">
				<view class="item flexRowBetween" v-for="item in mainData">
					<view class="pic">
						<image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow fs13 mgt5">{{item.product&&item.product.title?item.product.title:''}}</view>
						<view class="flexRowBetween B-price">
							<view class="price fs14">{{item.product&&item.product.title?item.product.price:''}}</view>
							<view class="flexEnd">
								<view class="numBox flex">
									<view @click="counter('-')">-</view>
									<view class="num pubColor">{{item.count}}</view>
									<view @click="counter('+')">+</view>
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
							<input type="text" v-model="wechat" placeholder="请输入微信号,以便客服联系(选填)" placeholder-class="placeholder" />
						</view>
					</view>
				</view>
			</view>

		</view>

		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween" style="height: 100rpx;">
			<view class="left flex mgl15">
				<view class="fs16 price">{{totalPrice}}</view>
			</view>
			<button class="payBtn fs16 white" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">确认预约</button>
		</view>
		<!-- 底部菜单按钮 end -->


	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils:this.$Utils,
				showView: false,
				wx_info: {},
				is_show: false,
				is_tmptShow: false,
				count: 1,
				wechat:'',
				mainData:[],
				addressData:{},
				totalPrice:0,
				pay:{},
			}
		},
		onLoad() {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			self.countTotalPrice()
		},

		onShow() {
			const self = this;
			if (uni.getStorageSync('choosedAddressData')) {
				self.addressData = uni.getStorageSync('choosedAddressData')

			} else {
				self.getAddressData()
			};
		},

		methods: {
			

			getAddressData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault: 1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
					}
				};
				self.$apis.addressGet(postData, callback);
			},

			counter(type) {
				const self = this;
				if (type == '+') {
					self.mainData[0].count++;
				} else {
					if (self.mainData[0].count > 1) {
						self.mainData[0].count--;
					}
				};
				self.countTotalPrice();
			},

			countTotalPrice() {
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
			},

			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);


				if (JSON.stringify(self.addressData) == '{}') {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请选择收货地址', 'none')
					return
				};
				/* if (self.wechat == '') {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入微信号', 'none')
					return
				}; */
				var data = {
					wechat:self.wechat
				}
				var orderList = [{
					product_id: self.mainData[0].product_id,
					count: self.mainData[0].count,
					type: 1,
					data: data,
					snap_address: self.addressData
				}];
				const callback = (user, res) => {
					self.addOrder(orderList)
				};
				self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList) {
				const self = this;	
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.goPay()
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay(order_id) {
				const self = this;	
				uni.setStorageSync('canClick',false);
				
				const postData = self.$Utils.cloneForm(self.pay)	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
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
										
										self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
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
								
								self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
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
	button{
		background: none;
		line-height: 1.5;
		margin-left: 0;
		margin-right: 0;
		font-size: 16px;
		border-radius: 0;
	}
	button::after{
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
