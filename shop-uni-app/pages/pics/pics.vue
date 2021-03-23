<template>
	<view class="content">
		<scroll-view scroll-y class="left">
			<view v-for="(item, index) in categories" :class="active===index?'active':''"  :key="index" @click="clickLeft(index, item.id)">{{ item.title }}</view>
		</scroll-view>
		<scroll-view scroll-y class="right">
			<view  v-for="(item,index) in rightContent" :key="index">
				<view>
					<image :src="item.img_url" mode="scaleToFill"></image>
				</view>
				<view >
					<image :src="require('../../static/logo.png')" @click="previewImg()"></image>
				</view>
				<view class="title">
					{{item.title}}
				</view>
				<view >
					{{item.zhaiyao}}
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
export default {
	data() {
		//const nullImage = require('@/static/error_img.png')
		return {
			categories: [],
			rightContent: [],
			active:0,
			//errorImg: 'this.src="' + nullImage + '"',
			sampleImg:'../../static/logo.png'
		};
	},
	onLoad() {
		this.getCategory();
	},
	methods: {
		async getCategory() {
			const res = await this.$myRequest({
				url: '/api/getimgcategory'
			});
			this.categories = res.data.message;
			console.log(this.categories);
			this.clickLeft(0, this.categories[0].id);
		},
		async clickLeft(index, id) {
			this.active=index;
			const res = await this.$myRequest({
				url: '/api/getimages/' + id
			});
			this.rightContent = res.data.message;
			console.log(this.rightContent);
		},
		previewImg(){ //path
			let imageUrl= this.sampleImg;
			console.log(imageUrl) ;
			        var images = [];
			        images.push(imageUrl);
			        console.log(images)   
			        uni.previewImage({ // 预览图片  图片路径必须是一个数组 => ["http://192.168.100.251:8970/6_1597822634094.png"]
			            current:0,
			            urls:images,
			            longPressActions: {  //长按保存图片到相册
			                itemList: ['保存图片'],
			                success: (data)=> {
			                    console.log(data);
			                    uni.saveImageToPhotosAlbum({ //保存图片到相册
			                        filePath: payUrl,
			                        success: function () {
			                            uni.showToast({icon:'success',title:'保存成功'})
			                        },
			                        fail: (err) => {
			                            uni.showToast({icon:'none',title:'保存失败，请重新尝试'})
			                        }
			                    });
			                },
			                fail: (err)=> {
			                    console.log(err.errMsg);
			                }
			        }
			        });
		}
	}
};
</script>

<style lang="scss">
page {
	height: 100%;
}
.content {
	display: flex;
	flex-direction: row;
	height: 100%;
	.left {
		width: 30%;
		.active{
			color: white;
			background-color: $theme-color;
		}
		view {
			height: 60px;
			line-height: 60px;
			text-align: center;
			font-size: 30rpx;
			border-top: 1px solid $backgroung-grey;
		}
	}
	.right {
		width: 70%;
		margin: 0 5px;
		.title{
			font-size: large;;
			font-weight: bold;
			color: $theme-color;
			margin: 5px 0;
			}
	}
}
image{
	background-image: url(../../static/error_img.png);
	background-size: cover;
}
</style>
