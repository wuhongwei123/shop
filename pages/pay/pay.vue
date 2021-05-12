<template>
	<view class="page">
		<view class="revice_address_row">
			
		</view>
		<view class="cart_content">
			<view class="cart_title">支付界面</view>
			<view class="cart_main">
				<!-- 当cart数组长度不为0显示商品信息 -->
					<view class="cart_item"
					v-for="(item,index) in cart" :key="index">
						
						<!-- 商品图片 -->
						<view class="cart_img_wrap">
							<navigator class="cart_img_wrap">
								<image :src="item.data.message.goods_small_logo" mode="widthFix"></image>
							</navigator>
						</view>
						<!-- 商品信息 -->
						<view class="cart_info_wrap">
							<view class="goods_name">{{item.data.message.goods_name}}</view>
							<view class="goods_price_wrap">
								<view class="goods_price">￥{{item.data.message.goods_price}}</view>
								<view class="cart_num_tool">
					
									<view class="goods_num">x{{item.num}}</view>
									
								</view>
							</view>
						</view>
					</view>			
			</view>
		</view>
	    <view class="footer_tool">
			<!-- 总价格 -->
			<view class="total_price_wrap">
				<view class="total_price">
					合计: <text class="total_price_text">￥{{totalPrice}}</text>
				</view>
				<view>包含运费</view>
			</view>
			<view class="order_pay_wrap" @click="handleOrderPay">
				支付({{totalNum}})
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cart:[],
				totalPrice: 0,
				totalNum: 0
			}
		},
	
		onShow() {
			// 1.获取缓存中的购物车数据
			let cart = uni.getStorageSync("cart") || [];
			// 过滤后的购物车数组
			cart = cart.filter(v=>v.checked)
	        // 1.总价格 总数量
	        	let totalPrice = 0;
	        	let totalNum = 0;
	        	cart.forEach(v=>{
	        		totalPrice+=v.num*v.data.message.goods_price;
	        		totalNum+=v.num;	
	        	});
	        	this.cart = cart;
	        	this.totalNum = totalNum;
	        	this.totalPrice = totalPrice;
	        },
			methods:{
			// async	handleOrderPay() {
					// try {
						// 1.判断有无token
						// const token = uni.getStorageSync("token")
						// if(!token) {
						// 	uni.navigateTo({
						// 		url: '/pages/auth/auth'
						// 	})
						// 	return
						// }
						// 2.创建订单 准备请求头参数
						// const　header = {Autherization:token}
						// const order_price = this.data.totalPrice;
						// const consignee_addr = this.data.address.all
						// let goods=[];
						// cart.forEach(v=>goods.push({
						// 	goods_id:v.goods_id,
						// 	goods_number:v.num,
						// 	goods_price:v.goods_price
						// }))
						// const orderParams = {order_price,consignee_addr,goods_price}
						// 3.准备发送请求 创建订单 
						// const {order_number} = await this.$myRequest({
						// 	url: '/v1/my/orders/req_unifiedorder',
						// 	methods: "POST",
						// 	data: orderParams,
						// 	header
						// })
						// 4.发起 支付接口
						// const res = await this.$myRequest({
						// 	url: "/v1/my/orders/req_unifiedorder",
						// 	methods: "POST",
						// 	data: {order_number},
						// 	header
						// })
						// 5.发起微信支付
						// uni.requestPayment({
						// 	timeStamp:'',
						// 	nonceStr:'',
						// 	package:'',
						// 	paySign:'',
						// 	success: (res)=> {
								
						// 	},
						// 	fail: ()=> {},
						// 	complete:()=> {}
						// })
						// 6.查看订单支付状态
						// const res = await this.$myRequest({
						// 	url: "/v1/my/orders/chkOrder",
						// 	methods: "POST",
						// 	head,
						// 	data: {order_number}
						// })
						// uni.showToast({title:"支付成功"})
						// 7.手动删除已经支付的商品
						// let newCart=uni.getStorageSync("cart")
						// newCart=newCart.filter(v=>!v.checked)
						// uni.setStoraageSync("cart",newCart)
						// 8.支付成功跳转到订单页面
						// uni.navigateTo({
						// 	url:'/pages/order/order'
						// })
					// } catch(error) {
						// await showToast({title:"支付失败"})
					// }
				// }
			}
		}
		
</script>

<style lang="less" scoped>
 .revice_address_row {
	 
 }
 .cart_content {
	 .cart_title{
		 padding: 20rpx;
		 font-size: 36rpx;
		 color: red;
		 border-top: 1rpx solid currentColor;
		 border-bottom: 1rpx solid currentColor;
	 }
	 .cart_main {
		 .cart_item{
			 display: flex;
			 padding: 10rpx ;
			 border-bottom: 1rpx solid #ccc;
		 		
		 		 .cart_img_wrap {
					 flex: 2;
					 display: flex;
					 justify-content: center;
					 align-items: center;
		 			 .cart_img_wrap {
		 				 image {width: 80%;}
		 			 }
		 		 }
		 		 .cart_info_wrap {
					 flex: 4;
					 display: flex;
					 flex-direction: column;
					 justify-content: space-around;
		 			 .goods_name {
		 				 display: -webkit-box;
						 overflow: hidden;
                         -webkit-box-orient:vertical ;
						 -webkit-line-clamp:2;
						 color: #666;
		 			 }
		 			 .goods_price_wrap {
						 display: flex;
						 justify-content: space-between;
		 				 .goods_price {
							 color: red;
							 font-size: 34rpx;
						 }
		 				 .cart_num_tool {
							 display: flex;
		 					 
		 					 .goods_num {
								width: 55rpx;
								height: 55rpx;
								display: flex;
								justify-content: center;
								align-items: center;
							 }
		 					
		 				 }
		 			 }
		 		 }
		 }
	 }
 }
 .page {
	 padding-bottom: 90rpx;
 }
 .footer_tool {
	 position: fixed;
	 bottom: 0;
	 left: 0;
	 width: 100%;
	 height: 90rpx;
	 background-color: #fff;
	 display: flex;
	 border-top: 1rpx solid #ccc;
	
	 .total_price_wrap {
		 flex: 5;
		 padding-right: 15rpx;
		 text-align: right;
		 font-size: 34rpx;
	 	 .total_price {
			 .total_price_text {
			 			 color: red;
			 			 font-size: 34rpx;
			 			 font-weight: 600;
			 }
		 }
	 }
	 .order_pay_wrap{
		 flex: 3;
		 background-color: red;
		 font-size: 32rpx;
		 font-weight: 600;
		 display: flex;
		 justify-content: center;
		 align-items: center;
	 }
 }
 
</style>
