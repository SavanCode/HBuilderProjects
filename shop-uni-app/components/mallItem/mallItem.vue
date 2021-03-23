<template>
	<view class="GoodList">  
		<view class="goodItem" v-for="item in itemsList" :key="item.id" @click="goDetail(item.id)"> 
			<image
				style="width: auto;min-width:140px; height: 200px;"
				src="https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=357454225,3085291841&fm=26&gp=0.jpg"
				mode="scaleToFill"
			></image>
			<view class="price">
				<text class="currentPrice">{{ item.sell_price }}</text>
				<text class="historyPrice">{{ item.market_price }}</text>
			</view>
			<view class="name">{{ item.title }}</view>
		</view>
	</view>
</template>

<script>
export default {
	props:{
		itemsList:Array
	},
	data() {
		return { 
		};
	},
	methods:{
		goDetail(id){
			console.log(id)
			uni.navigateTo({
				url:'../../pages/good-detail/good-detail?id='+id
			})
		}
	}
};
</script>

<style>
@import '../../static/iconfont/iconfont.css';
</style>
<style lang="scss">
.GoodList {
	background-color: $backgroung-grey;
	display: flex;
	justify-content: space-evenly;
	flex-flow: wrap;
	.goodItem {
		padding: 10rpx;
		margin-bottom: 15rpx;
		max-width: 45%;
		background-color: white;
		.price {
			font-size: 18px;
			color: red;
			padding: 8px 0;
			.historyPrice {
				color: $backgroung-grey;
				text-decoration: line-through;
				margin-left: 10px;
				font-size: 13px;
			}
		}
		.name {
			font-size: 14px;
		}
	}
}
</style>
