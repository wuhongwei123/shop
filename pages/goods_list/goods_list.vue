<template>
	<view class="tabs">
		<view class="tabs_title">
			<view class="title_item"
			@click="titleClick(index)"
			v-for="(item,index) in tabs" 
			:key="index"
			:class="{active: index === currentIndex}">
				{{item}}
			</view>
		</view>
		<view class="tabs_content">
			<view class="first_tab">
				<navigator class="goods_item" 
				v-for="(item,index) in QueryParams.goods"
				:url="'/pages/goods_detail/goods_detail?goods_id='+item.goods_id">
					<view class="goods_img_wrap">
						<image :src="item.goods_small_logo?item.goods_small_logo:'https://ww1.sinaimg.cn/large/007rAy9hgy1g24by9t530j30i20i2glm.jpg'" mode="widthFix"></image>
					</view>
					<view class="goods_info_wrap">
						<view class="goods_name">{{item.goods_name}}</view>
						<view class="goods_price">￥{{item.goods_price}}</view>
					</view>
				</navigator>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				tabs:['综合','销量','价格'],
				currentIndex: 0,
				QueryParams: {
					query: "",
					cid: "",
					pagenum: 1,
					pagesize:10
				},
				totalPages: 0,
			}
		},
		onLoad (options) {
			console.log(options.cid)
			this.QueryParams.cid = options.cid;
			this.getGoodsList();
		},
		onReachBottom() {
			if(this.QueryParams.pagenum>=this.totalPages) {
				uni.showToast({
					title: '没有下一页数据了'
				});
			} else {
				this.QueryParams.pagenum++;
				console.log(this.QueryParams.pagenum)
				this.getGoodsList()
			}
		},
		onPullDownRefresh() {
			// 1.重置数组
			this.QueryParams.pagenum = 1
			this.getGoodsList()
			// 2.关闭下拉
			uni.stopPullDownRefresh()
		},
		methods:{
			titleClick(index) {
				this.currentIndex = index
			},
			// 获取商品列表数据
			async getGoodsList() {
				const res = await this.$myRequest({
					url: '/v1/goods/search',
					data: this.QueryParams
				})
				// 获取总条数
				const total = res.data.message.total
				this.QueryParams = res.data.message
				this.totalPages = Math.ceil(total/10)
				// console.log(this.QueryParams)
				// console.log(this.totalPages)
		    },
			
		}
	}
</script>
 
<style lang="less" scoped>
	.tabs{}
	.tabs_title {
		display: flex;
	}
	.title_item {
		display: flex;
		justify-content: center;
		align-items: center;
		flex: 1;
		padding: 15rpx 0;
	}
	.active {
		color: red;
		border-bottom: 5rpx solid currentColor;
	}
	.tabs_content{}
	
	.tabs_content {
		.first_tab{
			.goods_item {
		        display: flex;
				border-bottom: 1rpx solid #ccc;
				.goods_img_wrap {
					flex:2;
					display: flex;
					justify-content: center;
					align-items: center;
					image {
						width: 70%;
					}
				}
			}
			.goods_info_wrap{
				flex: 3;
				display: flex;
				flex-direction: column;
				justify-content: space-around;
				.goods_name{
					display: -webkit-box;
					overflow: hidden;
					-webkit-box-orient:vertical;
					-webkit-line-clamp:2;
				}
				.goods_price{
					color: red;
					font-size: 32rpx;
				}
			}
		}
	}
</style>
