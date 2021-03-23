<template>
	<view>  
		<mall-item :itemsList="goodList"></mall-item>
		<view class="isOver" v-if="flag">----------我是有底线的----------</view>
	</view>
</template>

<script>
import mallItem from '@/components/mallItem/mallItem.vue';
export default {
	data() {
		return {
			goodList: [],
			pageindex:1,
			flag:false
		};
	},
	components: {
		'mall-item': mallItem
	},
	onLoad() {
		this.getGoods();
	},
	methods: {
		async getGoods() {
			const res = await this.$myRequest({
				url: '/api/getgoods?pageindex='+this.pageindex
			});
			this.goodList = [...this.goodList,...res.data.message];
		},
		onReachBottom() {
			console.log("触底")
			if(this.goodList.length<this.pageindex*10) return this.flag = true
			this.pageindex++
			this.getGoods()
		},
		onPullDownRefresh() {
		  this.goodList = []
		  this.pageindex = 1
		  this.flag = false 
		  setTimeout(()=>{
		    this.getGoods(()=>{
		      uni.stopPullDownRefresh()
		    })
		  },1000)
		}
	}
};
</script>

<style lang="scss">
	.isOver {
		width: 100%;
		height: 50px;
		line-height: 50px;
		text-align: center;
		background: $backgroung-grey;
		font-size: 28rpx;
	}
</style>
