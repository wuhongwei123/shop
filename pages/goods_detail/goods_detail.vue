<template>
	<view class="page">
		<view class="detail_swiper">
			<swiper autoplay circular indicator-dots>
				<swiper-item v-for="(item,index) in goodsObj.data.message.pics" :key="index">
					<image 
					:src="item.pics_mid" 
					mode="widthFix"
					@click="handlePreviewImage(index)"></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="goods_price">￥{{goodsObj.data.message.goods_price}}</view>
		<view class="goods_name_row">
			<view class="goods_name">{{goodsObj.data.message.goods_name}}</view>
			<view class="goods_collect">
				<text class="iconfont icon-shoucang"></text>
				<view class="collect_text">收藏</view>
			</view>
		</view>
		<view class="goods_info">
			<view class="goods_info_title">图文详情</view>
			<view class="goods_info_content">
				<rich-text :nodes="goodsObj.data.message.goods_introduce"></rich-text>
			</view>
		</view>
		<view class="btm_tool">
			<view class="tool_item">
				<view class="iconfont icon-kefu"></view>
				<view>客服</view>
				<button open-type="contact"></button>
			</view>
			<view class="tool_item">
				<view class="iconfont icon-fenxiang"></view>
				<view>分享</view>
				<button open-type="share"></button>
			</view>
			<navigator  url="/pages/cart/cart" open-type="switchTab"
			class="tool_item">
				<view class="iconfont icon-qicheqianlian-select"></view>
				<view>购物车</view>
			</navigator>
			<view class="tool_item btn_cart" @click="cartClick">加入购物车</view>

			<view class="tool_item btn_buy">立即购买</view>
			
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				goodsObj: {},
				GoodsInfo:{}
			}
		},
		methods: {
			async getGoodsDetail(goods_id) {
				const goodsObj = await this.$myRequest({
					url: '/v1/goods/detail',
					data: {goods_id},
					
				})
				this.GoodsInfo = goodsObj;
				 this.goodsObj = goodsObj
				 console.log(this.goodsObj);
			},
			handlePreviewImage(index) {
				const urls = this.GoodsInfo.data.message.pics.map(v=>v.pics_mid);
				// 接受参数
				uni.previewImage({
					current: urls[index],
					urls: urls
				})
			},
			cartClick() {
				// 1.获取缓存中的数组
				let cart = uni.getStorageSync("cart")||[];
				// 2.判断 商品对象是否存在于购物车数组中
				let index = cart.findIndex(v=>v.goods_id===this.GoodsInfo.data.message.goods_id);
				if(index===-1) {
					// 不存在 第一次添加
					this.GoodsInfo.num = 1;  
					this.GoodsInfo.checked= true;
					cart.push(this.GoodsInfo)
				} else {
					// 已经存在购物车数据 执行num++
					cart[index].num++;
				}
				// 把购物车重新添加到缓存中
				uni.setStorageSync("cart",cart);
				// 弹窗提示
				uni.showToast({
					title:'加入成功',
					icon: 'success',
					// 防止用户手抖 1.5秒后才能再点
					mask: true,
				})
			}
		},
		onLoad(options) {
			const {goods_id} = options
			console.log(goods_id),
			this.getGoodsDetail(goods_id)
		}
	}
</script>

<style lang="less">
	page {
		padding-bottom: 90rpx;
	}
  swiper {
	  height: 60vw;
	  text-align: center;
	  image {
		  width: 60%;
	  }
  }
  .goods_price {
	  padding: 15rpx;
	  font-size: 32rpx;
	  font-weight: 600;
	  color: red;
  }
  .goods_name_row {
	  display: flex;
	  border-top: 5rpx solid #dedede;
	  border-bottom: 5rpx solid #dedede;
	  .goods_name{
		  flex: 5;
		  color: #000;
		  font-size: 30rpx;
		  padding: 0 10rpx;
		  
		  display: -webkit-box;
		  overflow: hidden;
		  -webkit-box-orient: vertical;
		  -webkit-line-clamp:2
	  }
	  .goods_collect {
		  flex: 1;
		  display: flex;
		  flex-direction: column;
		  justify-content: center;
		  align-items: center;
		  border-left: 1rpx solid #ccc;
		  .iconfont {}
		  .collect_text{}
	  }
	  .goods_info{
		  .goods_info_title {
			  font-size: 32rpx;
			  color: red;
			  font-weight: 600;
			  padding: 20rpx;
		  }
	  }
  }
  .btm_tool{
	  border-top: 1rpx solid #ccc;
	  position: fixed;
	  left: 0;
	  bottom: 0;
	  width: 100%;
	  height: 90rpx;
	  background-color: #fff;
	  display: flex;
	  .tool_item{
		  flex: 1;
		  display: flex;
		  flex-direction: column;
		  justify-content: center;
		  align-items: center;
		  font-size: 24rpx;
		  position: relative;
		  button {
			  position: absolute;
			  top: 0;
			  left: 0;
			  width: 100%;
			  height: 100%;
			  opacity: 0;
		  }
	  }
	  .btn_cart{
		  flex: 2;
		  display: flex;
		  flex-direction: column;
		  justify-content: center;
		  align-items: center;
		  background-color: #ffa500;
		  color: #fff;
		  font-size: 30rpx;
		  font-weight: 600;
	  }
	  .btn_buy{
		  flex: 2;
		  display: flex;
		  flex-direction: column;
		  justify-content: center;
		  align-items: center;
		  background-color: #eb4450;
		  color: #fff;
		  font-size: 30rpx;
		  font-weight: 600;
	  }
  }
</style>
