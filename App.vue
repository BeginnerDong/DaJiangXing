
<script>
	import token from "./common/token.js"
	export default {
		onLaunch: function() {	
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	@import "/assets/style/style.css";
	@import "/assets/style/quill.css";
	
	uni-swiper .uni-swiper-dots-horizontal{line-height: 16rpx;bottom: 5px;}
	
	uni-tabbar .uni-tabbar__icon{width: 44rpx;height: 44rpx;}
	uni-page-head .uni-page-head__title{font-weight: normal;}
	uni-page-head .uni-page-head{border-bottom: 1px solid #eee;}
	uni-swiper .uni-swiper-dot{width: 18rpx;height: 6rpx;border-radius: 6rpx; }
	
	.ql-editor img{max-width: 100%;margin: 0 auto;}
	
</style>
