<template>
	<view class="news">
		<news-item @itemClick="goDetail" :list="newsList"></news-item>
	</view>
</template>

<script>
	import newsItem from '../../components/news-item/news-item.vue'
	export default {
		data() {
			return {
				newsList: []
			}
		},
		components: {"news-item":newsItem},
		onLoad () {
			this.getNews()
		},
		methods: {
			async getNews () {
					const res = await this.$myRequest({
						url: '/api/getnewslist'
					})
					this.newsList = res.data.message
				},
				goDetail(id) {
					console.log(id)
					uni.navigateTo({
						url: '/pages/news_detail/news_detail'
						//url: '/pages/news_detail/news_detail?id'+id
					})
				}
			}
	}
</script>

<style>

</style>
