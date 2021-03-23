<template>
	<view class="wrapper">
		<view>
			<swiper indicator-dots>
				<swiper-item v-for="(item, index) in swipers" :key="index"><image :src="item.src"></image></swiper-item>
			</swiper>
		</view>
		<view class="content">
			<view class="info">
				<view class="info-price">
					<text>{{ info.sell_price }}</text>
					<text>{{ info.market_price }}</text>
				</view>
				<view class="info-title">{{ info.title }}</view>
			</view>
			<view class="info-more">
				<view class="info-num">
					<text>货号：{{ info.goods_no }}</text>
				</view>
				<view class="info-stock">库存：{{ info.stock_quantity }}</view>
			</view>
			<view class="info-detail">
				<text>商品详情</text>
				<view class="info-content"><rich-text :nodes="content"></rich-text></view>
			</view>
		</view>
		<view class="good-nav">
			<uni-goods-nav :fill="true" :options="options" :buttonGroup="buttonGroup" @click="onClick" @buttonClick="buttonClick" />
		</view>
	</view>
</template>

<script>
import uniGoodsNav from '@/components/uni-goods-nav/uni-goods-nav.vue';
export default {
	components: { 'uni-goods-nav':uniGoodsNav },
	data() {
		return {
			id: 0,
			info: '',
			content: '',
			swipers: [],
			options: [
				{
					icon: 'headphones',
					text: '客服'
				},
				{
					icon: 'shop',
					text: '店铺',
					info: 2,
					infoBackgroundColor: '#007aff',
					infoColor: 'red'
				},
				{
					icon: 'cart',
					text: '购物车',
					info: 2
				}
			],
			buttonGroup: [
				{
					text: '加入购物车',
					backgroundColor: '#ff0000',
					color: '#fff'
				},
				{
					text: '立即购买',
					backgroundColor: '#ffa200',
					color: '#fff'
				}
			]
		};
	},
	onLoad(options) {
		//console.log(options)
		this.id = options.id;
		this.getDetailInfo();
		this.getDetailContent();
		this.getSwipers();
	},
	methods: {
		async getSwipers() {
			const res = await this.$myRequest({
				url: '/api/getthumimages/' + this.id
			});
			this.swipers = res.data.message;
		},
		async getDetailInfo() {
			const res = await this.$myRequest({
				url: '/api/goods/getinfo/' + this.id
			});
			console.log(res.data.message[0]);
			this.info = res.data.message[0];
		},
		async getDetailContent() {
			const res = await this.$myRequest({
				url: '/api/goods/getdesc/' + this.id
			});
			console.log(res.data.message[0]);
			this.content = res.data.message[0].content;
		},
		onClick(e) {
			uni.showToast({
				title: `点击${e.content.text}`,
				icon: 'none'
			});
		},
		buttonClick(e) {
			console.log(e);
			this.options[2].info++;
		}
	}
};
</script>

<style lang="scss">
.wrapper {
	background-color: $backgroung-grey;
}
swiper {
	width: 100%;
	height: 350rpx;
	image {
		width: 100%;
		height: 100%;
	}
}
.content {
	//padding: 5px;
}
.info-price {
	background-color: white;
	font-size: xx-large;
	color: red;
	text:nth-child(2) {
		color: $backgroung-grey;
		text-decoration: line-through;
		font-size: medium;
	}
}
.info-title {
	background-color: white;
	font-size: x-large;
	display: flex;
	flex-flow: wrap;
}
.info-more {
	background-color: white;
	margin: 5px 0;
}
.info-detail {
	background-color: white;
	text:nth-child(1) {
		font-size: large;
		color: $theme-color;
	}
	.info-content {
		line-height: 25px;
	}
}
.good-nav{
	position: fixed;
	bottom: 0;
	width: 100%;
}
image {
	background-image: url(../../static/error_img.png);
	background-size: cover;
	width: 100%;
}
</style>
