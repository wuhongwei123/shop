<template>
	<view>
		<button type="primary" plain open-type="getUserInfo" @click="hanleGetUserInfo">获取授权</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			// 获取token
			hanleGetUserInfo(e){
			// const {encryptedData,rawDate,iv,signature} = e.detail
			// uni.login({
			//   provider: 'weixin',
			//   success: function (loginRes) {
			//     const {code} = loginRes;
			//   }
			// });
			// const loginParams={encryptedData,rawDate,iv,signature,code}
			// const res = await this.$myRequest({
			// 	url: "/v1/users/wxlogin",
			// 	data: loginParams,
			// 	methods: "post"
			// })
			// uni.getStorageSync("token",token)
			// uni.navigateBack({
			// 	delta:1
			// })
			// }
			// uni.getStorageSync("token",token)
			// uni.navigateBack({
			// 		delta:1,
			//  })
			}
		}
	}
</script>

<style scoped>
  button {
	  margin-top: 40rpx;
	  width: 70%;
  }
</style>
