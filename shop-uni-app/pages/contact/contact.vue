<template>
	<view>
		<image class="about_us_img" src="http://www.itcast.cn/2018czydz/images/gywmban.jpg"></image>
		<view class="info">
			<view @click="phone">联系电话：400-618-9090 ( 点击拨打 )</view>
			<view>校区地址：浙江省杭州市下沙经济开发区4号大街187号盛泰时代山</view>
		</view>
		<view class="page-section page-section-gap">
			<map class="map" :latitude="latitude" :longitude="longitude" :markers="markers"></map>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			//id: 0, // 使用 marker点击事件 需要填写id
			//title: 'map',
			latitude: 39.909,
			longitude: 116.39742,
			scale:13,
			markers:[
				{
					longitude: 39.909,
					latitude: 116.39742,
					iconPath: '../../static/logo.png',
					width: 30,
					height: 30
				}
			]
		};
	},
	methods:{
		phone() {
			uni.makePhoneCall({
				phoneNumber: '400-618-9090'
			})
		}
	}
};
</script>

<style lang="scss">
.about_us_img {
	width: 100%;
}
.info {
	margin: 10px 5px;
	view {
		padding: 5px 0;
	}
}
.map{
	width: 95%;
	margin: 0 5px;
}
</style>
