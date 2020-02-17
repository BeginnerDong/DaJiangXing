<template>
	<view>
		
		<view class="pdlr4">
			<view class="seachBox mgt15 flexCenter color9 fs12" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				<view>搜索</view>
			</view>
			<view class="banner-box">
				<view class="banner pdtb15">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#ff5c11">
						<block v-for="(item,index) in sliderData.mainImg":key="index">
							<swiper-item class="swiper-item">
								<image :src="item.url" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="indHome flex fs14 pdt15">
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/product/product?name=水电'}})">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="tit">水电</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/product/product?name=家电'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">家电</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/product/product?name=灯具'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="tit">灯具</view>
			</view>
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/product/product?name=疏通'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="tit">疏通</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/product/product?name=家具'}})">
				<image src="../../static/images/home-icon5.png"></image>
				<view class="tit">家具</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/product/product?name=墙面'}})">
				<image src="../../static/images/home-icon6.png"></image>
				<view class="tit">墙面</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/product/product?name=地面'}})">
				<image src="../../static/images/home-icon7.png"></image>
				<view class="tit">地面</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/product/product?name=厨卫'}})">
				<image src="../../static/images/home-icon8.png"></image>
				<view class="tit">厨卫</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/product/product?name=门窗'}})">
				<image src="../../static/images/home-icon9.png"></image>
				<view class="tit">门窗</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/product/product?name=其他'}})">
				<image src="../../static/images/home-icon10.png"></image>
				<view class="tit">其他</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="mglr4 pdtb15">
			<view class="fs15 ftw pdb10">热门服务</view>
			<view class="flexRowBetween ind-service">
				<view class="item" v-for="(item,index) in mainData"   :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="title pdb5 avoidOverflow">{{item.title}}</view>
					<view class="text fs12 avoidOverflow color6">{{item.description}}</view>
					<view class="flexRowBetween infor">
						<view class="money">￥{{item.price}}</view>
						<view class="Rimg"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
					</view>
				</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">我的</view>
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
				wx_info:{},
				is_show:false,
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png"
				],
				serviceData:[
					{title:'马桶疏通',text:'使用专用工具对马桶内堵塞进行清理',imgUrl:'../../static/images/home-img1.png'},
					{title:'下水道疏通',text:'使用专用工具对下水道内堵塞进行清理',imgUrl:'../../static/images/home-img2.png'},
					{title:'洗衣机清洗',text:'使用专用工具对洗衣机内堵塞进行清理',imgUrl:'../../static/images/home-img3.png'},
					{title:'热水器清洗',text:'使用专用工具对热水器内堵塞进行清理',imgUrl:'../../static/images/home-img4.png'},
					{title:'电路检修',text:'使用专用工具对电路问题进行清理维修',imgUrl:'../../static/images/home-img5.png'},
					{title:'冰箱清洗',text:'使用专用工具对冰箱内外部进行清理维修',imgUrl:'../../static/images/home-img6.png'}
				],
				productData:[{},{},{},{},{},{}],
				sliderData:{},
				mainData:[],
				searchItem:{
					thirdapp_id:2
				}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getSliderData','getMainData','getUserInfoData'], self);
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
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					}
					console.log('self.userInfoData', self.userInfoData)
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					title:'首页轮播',
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.sliderData = res.info.data[0]
					}
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/index.css";
	
	page {padding-bottom: 140rpx;}

</style>
