<template>
	<view>
		<view class="mglr4 pdtb10">
			<view class="whiteBj radius10 mgb15">
				<view class="flex"><image class="w" style="" src="../../static/images/the-order-img.png" mode="widthFix"></image></view>
				<view class="flexRowBetween pdtb15 mglr4"  @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<view class="fs13">
						<view>陕西省西安市雁塔区高新大都荟</view>
						<view class="color6 flex mgt5">
							<view class="mgr10">张思德</view>
							<view class="">15689562352</view>
						</view>
					</view>
					<view class="flexEnd" style="width: 20%;">
						<image class="arrowR" src="../../static/images/about-icon6.png" mode=""></image>
					</view>
				</view>
			</view>
			
			<view class="proRow radius10 whiteBj mgb15">
				<view class="item flexRowBetween">
					<view class="pic">
						<image src="../../static/images/the-order-img1.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow fs13 mgt5">马桶堵塞</view>
						<view class="flexRowBetween B-price">
							<view class="price fs14">56.00</view>
							<view class="flexEnd">
								<view class="numBox flex">
									<view @click="counter('-')">-</view>
									<view class="num pubColor">{{count}}</view>
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
							<input type="text" value="" placeholder="请输入" placeholder-class="placeholder" />
						</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween" style="height: 100rpx;">
			<view class="left flex mgl15">
				<view class="fs16 price">856.00</view>
			</view>
			<view class="payBtn fs16 white">确认预约</view>
		</view>
		<!-- 底部菜单按钮 end -->
		
		 
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
				is_tmptShow:false,
				count:1
			}
		},
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
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
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/detail.css";
	page{background: #F5F5F5;padding-bottom: 120rpx;}

	.proRow .item{padding: 30rpx 4%;}
	.proRow .item .pic{width: 190rpx; height: 190rpx;}
	.proRow .item .pic image{width: 100%;height: 100%; display: block;}
	.proRow .item .infor{ width: 66%;height: 190rpx;position: relative;}
	.proRow .item .B-price{position: absolute;left: 0;right: 0;bottom: 10rpx;}
	
	.myRowBetween .item:last-child{border-bottom: 0;}
	
	/* 数量加减 */
	.numBox{border:1px solid #eee; border-radius: 8rpx;}
	.numBox view{ width: 44rpx; height: 44rpx;color: #999; font-size:32rpx ; text-align: center; line-height: 44rpx;}
	.numBox view.num{ width: 60rpx; color: #333; font-size: 26rpx;border-left:1px solid #eee;border-right:1px solid #eee;}
	
</style>

