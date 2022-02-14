<template>
	<view class="container">

		<view class="today">
			<image class="logo" src="../../static/home.png" mode=""></image> 欢迎来到小杜之家
		</view>
		<view class="tag-wrapper-1" @click="wxLogin_btn()">微信授权登录</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		methods: {
			wxLogin_btn() {
				var that = this
				wx.getUserProfile({
					desc: '用于完善会员资料', // 声明获取用户个人信息后的用途，后续会展示在弹窗中，请谨慎填写
					success: (res) => {
						console.log(res);
						uni.setStorageSync('userinfo',res.userInfo)
						uni.reLaunch({
							url:'./user'
						})
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.container {
		padding: 20px;
		font-size: 14px;
		line-height: 24px;
		text-align: center;

	}

	.today {
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 34rpx;
		width: 400rpx;
		height: 80rpx;
		border-radius: 40rpx;
		color: #000;
		line-height: 80rpx;
		text-align: center;
		// background-color: #18BC37;
		margin: 0 auto;

		.logo {
			width: 50rpx;
			height: 50rpx;
		}
	}

	.tag-wrapper-1 {
		box-sizing: border-box;

		margin: 0 auto;



		margin-top: 100rpx;
		border-radius: 16rpx;
		background-color: #0fbd5b;
		text-align: center;
		height: 80rpx;
		width: 400rpx;
		font-weight: 700;
		line-height: 80rpx;
		font-size: 34rpx;
		color: #ffffff;

	}
</style>
