<template>
	<view>
		<view class="mglr4 flexRowBetween productList pdtb15">
			<view class="item radius10" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
			@click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
				<view class="infor">
					<view class="tit avoidOverflow">{{item.title}}</view>
					<view class="price">{{item.price}}</view>
				</view>
				
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
				productData:[{},{},{},{},{}],
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.name = options.name;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			uni.setNavigationBarTitle({
			    title: self.name
			});
			self.$Utils.loadAll(['getMainData'], self);
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['in', [self.name]],
						},
						middleKey: 'category_id',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
					console.log('self.mainData', self.mainData)
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/product.css";
	page{padding-bottom: 40rpx;background: #F5F5F5;}
</style>

