<template>
	<view class="cates">
		<view class="cates_container" >
			<scroll-view class="left_menu" scroll-y>
				<view class="menu_item" 
				v-for="(item,index) in leftMenuList" 
				:key="index"
				@click="leftMenuClick(index)"
				:class="{active: index === currentIndex}">{{item}}</view>
			</scroll-view>
			<scroll-view class="right_content" scroll-y :scroll-top="scrollTop">
				<view class="goods_group" v-for="(item,index) in rightContent" :key="index">
					<view class="goods_title">
						<text class="delimiter">/</text>
						<text class="title">{{item.cat_name}}</text>
						<text class="delimiter">/</text>
					</view>
					<view class="goods_list">
						<navigator v-for="(item1,index) in item.children" 
						:key="index"
						:url="'/pages/goods_list/goods_list?cid='+item1.cat_id ">
							<image mode="widthFix" :src="item1.cat_icon"></image>
							<view class="goods_name">{{item1.cat_name}}</view>
						</navigator>
					</view>
				</view>
			</scroll-view>
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				leftMenuList:[],
				rightContent:[],
			    currentIndex: 0,
				scrollTop: 0,
			}
		},
		Cates: [],
		onLoad() {
			// 判断本地存储有无旧数据 有就使用没有请求
			 // this.getCates()
			// 1.获取本地存储数据
			const Cates = uni.getStorageSync("cates");
			if(!Cates) {
				this.getCates()
			} else {
				// 有旧数据 过期是时间
				if(Date.now()-Cates.time>10000) {
					this.getCates()
				} else {
					this.Cates = Cates.data
					let leftMenuList = this.Cates.map(v=>v.cat_name)
					let rightContent = this.Cates[0].children;
					this.leftMenuList = leftMenuList
					this.rightContent = rightContent
				}
			}
		},
		methods: {
			async getCates() {
				const res = await this.$myRequest({
					url: '/v1/categories'
				})
				this.Cates = res.data.message;
				uni.setStorageSync("cates",{time:Date.now(),data:this.Cates})
				 console.log(this.Cates)
				 // 构造左侧大菜单数据
				 let leftMenuList = this.Cates.map(v=>v.cat_name)
				 let rightContent = this.Cates[0].children;
				 this.leftMenuList = leftMenuList
				 this.rightContent = rightContent
			},
			leftMenuClick(index) {
				let rightContent = this.Cates[index].children;
				this.currentIndex = index			
				this.rightContent = rightContent
				// 重新设置 到顶部的距离
				this.scrollTop = 0
			}
			
			
		}
		
	}
</script>

<style lang="less">
 page {
 	height: 100%;
 }
 .cates{
 	height: 100%;
 	.cates_container{
		display: flex;
 		.left_menu{
			height: 100vh;
			flex: 2;
			.menu_item {
				height: 80rpx;
				display: flex;
				justify-content: center;
				align-items: center;
				font-size: 30rpx;	
			}
			.active {
				color: #B50E03;
				border-left: 5px solid currentColor
			}
			
		}
 		.right_content{
			flex: 5;
			height: 100vh;
			.goods_group {
				.goods_title{
					height: 80rpx;
					display: flex;
					justify-content: center;
					align-items: center;
					.delimiter{
						color: #ccc;
						padding: 0 10rpx;
					}
					.title {}	
				}	
				.goods_list{
					display: flex;
					flex-wrap: wrap;
					navigator {
						width: 33.33%;
						text-align: center;
						image{
							width: 50%;
						}
						.goods_name{}
					}
				}
			}
		}
 	}
 }
</style>
