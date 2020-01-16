<template>
	<view>
		<view class="mglr4 flexRowBetween productList pdtb15">
			<view class="item radius10" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
				<view class="pic"><image src="../../static/images/goods-img.png" mode=""></image></view>
				<view class="infor">
					<view class="tit avoidOverflow">洗衣机清洗</view>
					<view class="price">56</view>
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
				productData:[{},{},{},{},{}]
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
	@import "../../assets/style/product.css";
	page{padding-bottom: 40rpx;background: #F5F5F5;}
</style>

