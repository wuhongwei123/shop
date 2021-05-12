<template>
	<view class="page">	
		<view  class="user_info_wrap">
           <view v-if="userinfo.avatarUrl" class="user_img_wrap">
			   <image class="user_bg" :src="userinfo.avatarUrl" mode=""></image>
			  <view class="user_info">
				   <image class="user_icon" :src="userinfo.avatarUrl"></image>
				   <view class="user_name">{{userinfo.nickName}}</view>
			   </view>
		   </view>
		   <view v-else class="user_btn">
		   	<navigator url="/pages/login/login">登录</navigator>
		   </view>
		</view>	
	<view class="user_content">
		<view class="user_main">
			<!-- 历史足迹 -->
			<view class="history_wrap">
				<navigator>
					<view class="his_num">0</view>
					<view class="his_name">收藏的店铺</view>
				</navigator>
				<navigator>
					<view class="his_num">0</view>
					<view class="his_name">收藏的商品</view>
				</navigator>
				<navigator>
					<view class="his_num">0</view>
					<view class="his_name">关注的商品</view>
				</navigator>
				<navigator>
					<view class="his_num">0</view>
					<view class="his_name">我的足迹</view>
				</navigator>
			</view>
		    <!-- 我的订单 -->
			<view class="orders_wrap">
				<view class="orders_title">我的订单</view>
				<view class="order_content">
					<navigator >
						<view class="iconfont icon-wendang"></view>
						<view class="order_name">全部订单</view>
					</navigator>
					<navigator >
						<view class="iconfont .icon-fukuan"></view>
						<view class="order_name">待付款</view>
					</navigator>
					<navigator >
						<view class="iconfont .icon-kache"></view>
						<view class="order_name">带收货</view>
					</navigator>
					<navigator >
						<view class="iconfont .icon-up1"></view>
						<view class="order_name">退款/退货</view>
					</navigator>
				</view>
			</view>
	        <!-- 收货地址 -->
			<view class="address_wrap">
				收货地址管理
			</view>
			<!-- 应用信息相关 -->
			<view class="app_info_wrap">
				<view class="app_info_item  app_info_contact" >
					<text>联系客服</text>
					<text>400-618-4000</text>
				</view>
				<view class="app_info_item">意见反馈</view>
				<view class="app_info_item">关于我们</view>
			</view>
		    <!-- 推荐 -->
			<view class="recommend_wrap">
				把应用推荐给其他人
			</view>
		</view>
	</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userinfo:{}
			}
		},
		onShow() {
			const userinfo=uni.getStorageSync("userinfo");
			this.userinfo = userinfo
		},
		methods: {
			
		}
	}
</script>

<style scoped lang="less">
	.page {
		background-color: #edece8;
	}
  .user_info_wrap{
	  height: 45vh;
	  overflow: hidden;
	  background-color: #8b0000;
	  .user_img_wrap {
		  position: relative;
		  .user_bg{
			  height: 50vh;
			  // 高斯模糊
			  filter: blur(10rpx);
		  }
		  .user_info {
			  position: absolute;
			  left: 50%;
			  transform: translateX(-50%);
			  top: 30%;
			  text-align: center;
			  .user_icon {
				  width: 150rpx;
				  height: 150rpx;
				  border-radius: 50%;
			  }
			  .user_name {
				  color: #fff;
				  margin-top: 40rpx;
				  font-size: 40rpx;
			  }
		  }  
	  }
      .user_btn {
		  position: absolute;
		  left: 50%;
		  transform: translateX(-50%);
		  top: 17%;
		  border: 1rpx solid greenyellow;
		  color: greenyellow;
		  font-size: 38rpx;
		  padding: 30rpx;
		  border-radius: 10rpx;
	  }
  }
  
  .user_content {
	  position: relative;
	  .user_main {
		  color: #666;
		  padding-bottom: 100rpx;
		  position: absolute;
		  width: 90%;
		  left: 50%;
		  transform: translateX(-50%);
		  top: -40rpx;
		  .history_wrap {
			  background-color: #fff;
			  display: flex;
			  navigator {
				  flex: 1;
				  text-align: center;
				  padding: 10rpx 0;
				  font-size: 34rpx;
				  .his_num {
					  color: red;
				  }
			
			  }
		 
		  }
	      .orders_wrap {
			  background-color: #fff;
			  margin-top: 20rpx;
			  .orders_title{
				  padding: 20rpx;
				  border-bottom: 1rpx solid #ccc;
			  }
			  .order_content{
				  display: flex;
				  navigator {
					  flex: 1;
					  padding: 15rpx 0;
					  text-align: center;
					  font-size: 34rpx;
					  .iconfont{
						  color: red;
					  }
					  .order_name{}
				  }
			  }
		  }
	      .address_wrap {
			  margin-top: 30rpx;
			  background-color: #fff;
			  padding: 20rpx ;
		  }
		  .app_info_wrap {
			  margin-top: 30rpx;
			  background-color: #fff;
			 .app_info_item {
				 padding: 20rpx;
				 border-bottom: 1rpx solid #ccc;
			 }
			 .app_info_contact {
				 display: flex;
				 justify-content: space-between;
			 }
		  }
		  .recommend_wrap {
			  margin-top: 30rpx;
			  background-color: #fff;
			  padding: 20rpx;
		  }
	  }  
  }
  
  
</style>
