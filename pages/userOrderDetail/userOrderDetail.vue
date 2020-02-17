<template>
	<view>
		<view class="mglr4 pdtb10">
			<view class="whiteBj radius10 mgb15">
				<view class="flex"><image class="w" style="" src="../../static/images/the-order-img.png" mode="widthFix"></image></view>
				<view class="pdtb15 mglr4">
					<view class="fs13">
						<view>{{addressData.city+addressData.detail}}</view>
						<view class="color6 flex mgt5">
							<view class="mgr10">{{addressData.name}}</view>
							<view class="">{{addressData.phone}}</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="proRow radius10 whiteBj mgb15">
				<view class="item">
					<view class="flexRowBetween fs12 mgb10">
						<view class="color9">交易时间：{{mainData.create_time}}</view>
						<view class="red" v-if="mainData.pat_status==0">待支付</view>
						<view class="red" v-if="mainData.pat_status==1&&mainData.transport_status==0">待接单</view>
						<view class="red" v-if="mainData.pat_status==1&&mainData.transport_status==1">服务中</view>
						<view class="red" v-if="mainData.pat_status==1&&mainData.transport_status==2">已完成</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
							mainData.orderItem[0].snap_product.mainImg&&mainData.orderItem[0].snap_product.mainImg[0]?mainData.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow fs13 mgt5">{{mainData.title}}</view>
							<view class="flexRowBetween B-price">
								<view class="price fs14">{{mainData.price}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="whiteBj radius10">
				<view class="myRowBetween pdlr4">
					<view class="item flexRowBetween">
						<view class="ll fs13">微信号</view>
						<view class="rr fs12">{{mainData.wechat}}</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;"  v-if="mainData.pat_status==1&&mainData.transport_status==1">
			<button class="btn" type="button" @click="Utils.stopMultiClick(orderUpdate)">确认完成</button>
		</view>
		
		 
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:{},
				id:-1,
				addressData:{}
			}
		},
		onLoad(options) {
			const self = this;
			
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self)
		},
		
		methods: {
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.data = {
					transport_status:2
				};
				postData.searchItem = {
					id:self.id,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
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
						
						
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
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

