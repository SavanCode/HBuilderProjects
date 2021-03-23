<template>
	<view>
		this is detail api down
		{{detail}}
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:4,
				detail:{}
			};
		},
		onLoad(options){
			console.log("options",options)
			//this.id = options.id
			this.getNewsDetail()
		},
		methods:{
			async getNewsDetail () {
				const res = await this.$myRequest({
					url: '/api/getnew/'+this.id
				})
				
				console.log("this is res",res)
				this.detail = res.data.message[0]
			}
		}
	}
</script>

<style lang="scss">

</style>
