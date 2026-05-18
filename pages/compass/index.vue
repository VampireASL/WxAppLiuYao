<template>
	<view class="container">
		<view class="header">
			<view class="back-btn" @click="goBack">
				<text class="back-icon">←</text>
			</view>
			<text class="title">指南针</text>
			<view class="placeholder"></view>
		</view>

		<view class="content">
			<view class="compass-placeholder">
				<text class="placeholder-icon">🧭</text>
				<text class="placeholder-text">指南针功能开发中...</text>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	methods: {
		goBack() {
			uni.navigateBack();
		}
	}
}
</script>

<style scoped>
.container {
	min-height: 100vh;
	background: linear-gradient(180deg, #F2F2F7 0%, #E5E5EA 100%);
}

.header {
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 20rpx 30rpx;
	background-color: #FFFFFF;
	box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.06);
}

.back-btn {
	width: 80rpx;
	height: 80rpx;
	display: flex;
	align-items: center;
	justify-content: center;
}

.back-icon {
	font-size: 48rpx;
	color: #007AFF;
	font-weight: 300;
}

.title {
	font-size: 36rpx;
	font-weight: 700;
	color: #1C1C1E;
}

.placeholder {
	width: 80rpx;
}

.content {
	flex: 1;
	display: flex;
	align-items: center;
	justify-content: center;
	padding: 60rpx 30rpx;
}

.compass-placeholder {
	background-color: #FFFFFF;
	border-radius: 32rpx;
	padding: 80rpx 60rpx;
	text-align: center;
	box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
}

.placeholder-icon {
	display: block;
	font-size: 120rpx;
	margin-bottom: 30rpx;
}

.placeholder-text {
	font-size: 32rpx;
	color: #8E8E93;
	font-weight: 500;
}
</style>