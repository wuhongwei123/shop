<template>
	<view class="home">
		<swiper indicator-dots circular autoplay="true">
			<swiper-item v-for=" item in swipers" :key="item.goods_id">	
					<image :src="item.image_src" mode=""></image>
			</swiper-item>
		</swiper>
		<view class="nav">
			<view class="nav_item">
				<view v-for=" (item,index) in CateList" :key="index" >
					<image :src="item.image_src" mode="heightFix"></image>
				</view>
			</view>
		</view>
		
		<view class="floor">
			<view class="floor_group" v-for="(item,index) in FloorList" :key="index" >
				<view class="floor_title">
					<image :src="item.floor_title.image_src" mode="widthFix"></image>
				</view>
				<view class="floor_list">
					<navigator v-for="(item1,index) in item.product_list" :key="index">
						<image :src="item1.image_src" mode="widthFix"></image>
					</navigator>
				</view>
			</view>
		</view>
		
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				swipers: [],
				CateList: [],
				FloorList: []
			}
		},
		onLoad() {
			this.getSwipers()
			this.getCateList()
			this.getFloorList()
		},
		methods: {
			async getSwipers() {
				const res = await this.$myRequest({
					url: '/v1/home/swiperdata'
				})
				this.swipers = res.data.message
				// console.log(this.swipers)
			},
			async getCateList() {
				const res = await this.$myRequest({
					url: '/v1/home/catitems'
				})
				this.CateList = res.data.message
				// console.log(this.CateList)
			},
			async getFloorList() {
				const res = await this.$myRequest({
					url: '/v1/home/floordata'
				})
				this.FloorList = res.data.message
				console.log(this.FloorList)
			}
		}
	}
</script>

<style lang="scss" scoped>
	.home {
		swiper{
			width: 750rpx;
			height: 380rpx;
		}
		image {
			height: 100%;
			width: 100%;
		}
	}
	.nav_item {
		display: flex;
	}
	.nav_item image {
		height: 100px;
	}
	.floor_title image {
		width: 100%;
	}
	.floor_list navigator {
		float: left;
		width: 33.3%;
	}
	.floor_list navigator image {
		width: 100%;
		height: 100%;
	}
	navigator:nth-last-child(-n+4) {
		height: 27.7vw;
	}
</style>
