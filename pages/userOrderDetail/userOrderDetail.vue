<template>
	<view>
		<view class="mglr4 pdtb10">
			<view class="whiteBj radius10 mgb15">
				<view class="flex"><image class="w" style="" src="../../static/images/the-order-img.png" mode="widthFix"></image></view>
				<view class="pdtb15 mglr4">
					<view class="fs13">
						<view>陕西省西安市雁塔区高新大都荟</view>
						<view class="color6 flex mgt5">
							<view class="mgr10">张思德</view>
							<view class="">15689562352</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="proRow radius10 whiteBj mgb15">
				<view class="item">
					<view class="flexRowBetween fs12 mgb10">
						<view class="color9">交易时间：2018-08-30</view>
						<view class="red">待接单</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/the-order-img1.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow fs13 mgt5">马桶堵塞</view>
							<view class="flexRowBetween B-price">
								<view class="price fs14">56.00</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="whiteBj radius10">
				<view class="myRowBetween pdlr4">
					<view class="item flexRowBetween">
						<view class="ll fs13">微信号</view>
						<view class="rr fs12">15623256565</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="button">确认完成</button>
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
				is_tmptShow:false
			}
		},
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	/* @import "../../assets/style/detail.css"; */
	page{background: #F5F5F5;padding-bottom: 120rpx;}

	.proRow .item{padding: 30rpx 4%;}
	.proRow .item .pic{width: 190rpx; height: 190rpx;}
	.proRow .item .pic image{width: 100%;height: 100%; display: block;}
	.proRow .item .infor{ width: 66%;height: 190rpx;position: relative;}
	.proRow .item .B-price{position: absolute;left: 0;right: 0;bottom: 10rpx;}
	
	.myRowBetween .item:last-child{border-bottom: 0;}

	
</style>

