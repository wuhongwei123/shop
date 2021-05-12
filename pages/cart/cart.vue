<template>
	<view class="page">
		<view class="revice_address_row">
			<view class="address_btn">
				<button type="primary" 
			 @click="handleChooseAddress()"	plain="">获取收获地址</button>
			</view>
		</view>
		<view class="cart_content">
			<view class="cart_title">购物车</view>
			<view class="cart_main">
				<!-- 当cart数组长度不为0显示商品信息 -->
				<block v-if="cart.length!==0">
					<view class="cart_item"
					v-for="(item,index) in cart" :key="index">
						<!-- 复选框 -->
						<view class="cart_chk_wrap">
							<checkbox-group :data-id="item.data.message.goods_id" @change="handleItemChange">
								<checkbox :checked="item.checked"></checkbox>
							</checkbox-group>
						</view>
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
									<view @tap="handleItemNumEdit" :data-id="item.data.message.goods_id" :data-operation="-1" class="num_edit">-</view>
									<view class="goods_num">{{item.num}}</view>
									<view @tap="handleItemNumEdit" :data-id="item.data.message.goods_id" :data-operation="1" class="num_edit">+</view>
								</view>
							</view>
						</view>
					</view>
					
				</block>
			    <block v-else>
					<image src="../../static/tabbar1/gouwuche.jpg" mode=""></image>
				</block>
			</view>
		</view>
	    <view class="footer_tool">
			<!-- 全选 -->
			<view class="all_chk_wrap">
				<checkbox-group @change="handleItemAllCheck">
					<checkbox :checked="allChecked"></checkbox>
				</checkbox-group>
			</view>
			<!-- 总价格 -->
			<view class="total_price_wrap">
				<view class="total_price">
					合计: <text class="total_price_text">￥{{totalPrice}}</text>
				</view>
				<view>包含运费</view>
			</view>
			
			<!-- 结算 -->
			<view class="order_pay_wrap" @click="handlePay">
				结算({{totalNum}})
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				cart:[],
				allChecked: false,
				totalPrice: 0,
				totalNum: 0
			}
		},
		onShow() {
			// 1.获取缓存中的购物车数据
			const cart = uni.getStorageSync("cart") || [];
			// every 数组方法 会遍历 接收一个回调函数那么 每一个回调函数都返回true 那么every才返回true 有个false 就返回false
			// 空数组调用every会返回true 
			// const allChecked = cart.length?cart.every(v=>v.checked) : false
			this.setCart(cart)
			
		},
		methods: {
			// 
			handleChooseAddress() {
				uni.chooseAddress({
					success(res) {
					    console.log(res)
					  }
				})
			},
			setCart(cart) {
				let allChecked = true
				// 1.总价格 总数量
				let totalPrice = 0;
				let totalNum = 0;
				cart.forEach(v=>{
					if(v.checked) {
						totalPrice+=v.num*v.data.message.goods_price;
						totalNum+=v.num;
					} else {
						allChecked = false
					}
				})
				allChecked = cart.length!=0?allChecked:false;
				this.cart = cart
				this.totalNum = totalNum;
				this.totalPrice = totalPrice
				this.allChecked = allChecked
				uni.setStorageSync("cart",cart);
			},
			// 商品的全选功能
			handleItemAllCheck() {
				// 1.获取data中的数据
			    let	cart = this.cart
			    let allChecked = this.allChecked
			    // 2.修改值 
				allChecked=!allChecked
				// 3.循环修改cart数组 中的商品
				cart.forEach(v=>v.checked=allChecked)
				// 4.修改后的值填充回data中 缓存中
				this.setCart(cart)
				
			},
			handleItemNumEdit(e){
	
				const {operation,id} = e.currentTarget.dataset;
				let cart = this.cart;
				const index = cart.findIndex(v=>v.data.message.goods_id===id)
				cart[index].num+=operation;
				this.setCart(cart)
				// 判断是否要删除
				if(cart[index].num===1 && operation===-1) {
					uni.showModal({
					    title: '提示',
					    content: '您是否要删除',
					    success:  (res) => {
					        if (res.confirm) {
					           cart.splice(index,1);
							   // this 指向 改成箭头函数解决
							   this.setCart(cart)
					        } else if (res.cancel) {
					           // 修改数量
					           cart[index].num+=operation
					           // 设置回data 缓存
					           this.setCart(cart)
					        }
					    }
					});
				}
				
			},
			handleItemChange(e) {
				
				const goods_id = e.currentTarget.dataset.id;
				console.log(goods_id)
				// 获取购物车数组
				let cart = this.cart
				// 找到被修改的商品对象
				console.log(cart)
				let index = cart.findIndex(v=>v.data.message.goods_id===goods_id)
				// 选中状态取反
				console.log(index)
				cart[index].checked=!cart[index].checked;
				// 把购物车数据重新设置回data 和 缓存中
				this.setCart(cart)	
			},
			// 结算
			handlePay() {
				// 1.判断有无收货地址
				// 2.判断有无选购商品
				console.log(this.totalNum)
				if(this.totalNum===0) {
					uni.showToast({
						title: '您还没有选购商品',
					})
					return
				}
				// 3.跳转到支付页面
				uni.navigateTo({
					url: '/pages/pay/pay'
				})
			}
		},
	}
</script>

<style lang="less" scoped>
 .revice_address_row {
	 .address_btn{
		 padding: 20rpx;
		 button {
			 width: 60%;
		 }
	 }
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
		 		 .cart_chk_wrap {
					 flex: 1;
					 display: flex;
					 justify-content: center;
					 align-items: center;
		 			 checkbox-group {
		 				 .checkbox {}
		 			 }
		 		 }
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
		 					 .num_edit {
								 width: 55rpx;
								 height: 55rpx;
								 display: flex;
								 justify-content: center;
								 align-items: center;
								 border: 1rpx solid #ccc;
							 }
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
	 .all_chk_wrap {
		 flex: 2;
		 display: flex;
		 justify-content: center;
		 align-items: center;
	 }
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
