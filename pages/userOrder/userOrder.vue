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
				<view class="item" v-for="item in mainData">
					<view class="flexRowBetween fs12 mgb10">
						
						<view class="color9">交易时间：{{item.create_time}}</view>
						<view class="red" v-if="item.pat_status==0">待支付</view>
						<view class="red" v-if="item.pat_status==1&&item.transport_status==0">待接单</view>
						<view class="red" v-if="item.pat_status==1&&item.transport_status==1">服务中</view>
						<view class="red" v-if="item.pat_status==1&&item.transport_status==2">已完成</view>
					</view>
					<view class="flexRowBetween" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/userOrderDetail/userOrderDetail?id='+$event.currentTarget.dataset.id}})">
						<view class="pic">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
							item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow2">{{item.title}}</view>
							<view class="B-price">
								<view class="price fs16 ftw">{{item.price}}</view>
							</view>
						</view>
					</view>
					<div class="underBtn flexEnd" v-if="item.pay_status==0">
						<span class="Bbtn" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/userOrderConfirm/userOrderConfirm?id='+$event.currentTarget.dataset.id}})">去支付</span>
					</div>
					<div class="underBtn flexEnd" v-if="item.pat_status==1&&item.transport_status==1"> @click="alertShow(item.id)">
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
				<view class="item pubColor"  @click="Utils.stopMultiClick(orderUpdate)">确定</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:[],
				current:1,
				is_alertShow:false,
				searchItem:{
					type:1,
					thirdapp_id:2
				},
				willId:-1
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			if(options.current){
				self.currentNew = options.current;
			}
		},
		
		onShow() {
			const self = this;
			if(self.currentNew&&self.currentNew!=1){
				self.change(self.currentNew)
			}else{
				self.mainData = [];
				self.getMainData(true)
			}
			
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
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
					id:self.willId,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current;
					if(self.current==1){
						delete self.searchItem.transport_status;
						delete self.searchItem.pay_status;
					}else if(self.current==2){
						self.searchItem.pay_status = 0
					}else if(self.current==3){
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 0
					}else if(self.current==4){
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 1;
						
					}else if(self.current==5){
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 2;
					}
					self.getMainData(true)
				}
			},
			
			alertShow(){
				const self=this;
				self.is_alertShow = !self.is_alertShow;
				self.is_show = !self.is_show;
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/myOrder.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	
</style>

