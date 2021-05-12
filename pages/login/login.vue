<template>
	<view>
		<button type="primary" open-type="getUserInfo" @getuserinfo="handlegetuserinfo" plain>登录</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: {
					avatarUrl:"http://img.houpao.com/dy/dyrs/20190907/c5360b8ab3229e87fbdca8ccdb8f296c.jpg",
					city: "Guangzhou",
					country: "China",
					gender:1,
					language:"zh_CN",
					nickName: "啦啦啦",
					province: "Guangdong"
				}
			} 
		},
		methods: {
			handlegetuserinfo(e) {
				uni.setStorageSync("userinfo",this.userInfo)
				uni.navigateBack({
					delta:1
				})
			}
		}
	}
</script>

<style>
    button {
	  margin-top: 40rpx;
	  width: 70%;
  }
</style>
