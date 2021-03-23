<template>
	<view>
		<view>
			<swiper class="swiper" indicator-dots :autoplay="true" :interval="2000" circular>
				<swiper-item v-for="item in swipers" :key="item.id"><image :src="item.img"></image></swiper-item>
			</swiper>
		</view>
		<view class="nav">
			<view class="nav-item" v-for="(item,index) in nav" :key="index" @click="goNavigator(item.path)">
				<!-- style="width: 120rpx;height:120rpx ;" -->
				<view :class="item.icons" :style="{ height: iconfontHeight }"></view>
				<!-- <view class="iconfont icon-geti" style="height: 200rpx;width: 200rpx;"></view> -->
				<text>{{item.title}}</text>
			</view>
		</view>
		<view class="recommendTitle"><view class="titleText">推荐商品</view></view>  
		<mall-item :itemsList="goodList" ></mall-item>
	</view>
</template>

<script>
import mallItem from '@/components/mallItem/mallItem.vue';
export default { 
	data() {
		return {
			swipers: [],
			goodList: [],
			iconfontHeight: '', 
			nav:[
				{
				    icons: "iconfont icon-geti",
				    title: "在线商城",
				    path: "/pages/onlineMall/onlineMall"
				  },
				  {
				    icons: "iconfont icon-laodonghetong",
				    title: "雄厚师资",
				    path: "/pages/contact/contact"
				  },
				  {
				    icons: "iconfont icon-ziliao",
				    title: "免费图库",
				    path: "/pages/pics/pics"
				  },
				  {
				    icons: "iconfont icon-shipin",
				    title: "网上课程",
				    path: "/pages/video/video"
				  }
			]
		};
	},
	onLoad() {
		this.getImgSwipers();
		this.getRecommendList();
	},
	onReady: function() {
		const query = uni.createSelectorQuery();
		let that = this;
		query
			.select('.iconfont')
			.boundingClientRect(data => {
				//这里必须写成data=>，而不能写成function(data)
				//console.log(data.width);
				that.iconfontHeight = data.width + 'px';
			})
			.exec();
	},
	components: {
		'mall-item': mallItem
	},
	methods: {
		async getImgSwipers() {
			const res = await this.$myRequest({
				methods: 'Get',
				url: '/api/getlunbo'
			});
			//console.log(res);
			this.swipers = res.data.message;
		},
		async getRecommendList() {
			const res = await this.$myRequest({
				methods: 'Get',
				url: '/api/getgoods?pageindex=1'
			});
			//console.log(res.data.message);
			this.goodList=res.data.message
		},
		goNavigator (url) {
		  uni.navigateTo({
		    url
		  })
		}
	}
};
</script>

<style>
@import '../../static/iconfont/iconfont.css';
</style>
<style lang="scss">
.swiper {
	height: 380rpx;
	image {
		width: 750rpx;
		height: 380rpx;
	}
}

.nav {
	display: flex;
	flex-direction: row;
	justify-content: space-around;
	text-align: center;
	margin: 20rpx 0;
	font-size: large;
	.nav-item {
		width: 20%;
		view {
			background-color: $theme-color;
			border-radius: 90px;
			display: flex;
			justify-content: center;
			align-items: center;
		}
		iconfont {
			width: 100%;
		}
	}
}
.recommendTitle {
	background-color: $backgroung-grey;
	padding: 5px;
	.titleText {
		font-size: larger;
		text-align: center;
		color: $theme-color;
		background-color: white;
	}
} 
</style>
