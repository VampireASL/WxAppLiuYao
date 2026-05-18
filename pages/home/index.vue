<template>
	<view class="container">
		<view class="header">
			<text class="title">传统文化</text>
			<text class="subtitle">经典文化传承</text>
		</view>

		<view class="content">
			<view class="intro-card">
				<text class="intro-text">
					这是一款传统文化学习工具，帮助用户了解和学习传统文化知识。
					通过有趣的互动方式，探索传统文化的魅力。
				</text>
			</view>

			<view class="btn-group">
				<view class="btn-wrapper" @click="goToCompass">
					<view class="btn btn-compass">
						<view class="btn-icon">🧭</view>
						<text class="btn-text">指南针</text>
					</view>
				</view>

				<view class="btn-wrapper" @click="goToLiuyao">
					<view class="btn btn-liuyao">
						<view class="btn-icon">✋</view>
						<text class="btn-text">天道</text>
					</view>
				</view>
			</view>
		</view>

		<view class="footer">
			<text class="footer-text">认真学习  收获快乐</text>
		</view>
	</view>
</template>

<script>
export default {
	methods: {
		goToCompass() {
			uni.navigateTo({
				url: '/pages/compass/index'
			});
		},
		goToLiuyao() {
			uni.navigateTo({
				url: '/pages/liuyao/index'
			});
		}
	}
}
</script>

<style scoped>
.container {
	min-height: 100vh;
	background: linear-gradient(180deg, #F2F2F7 0%, #E5E5EA 100%);
	display: flex;
	flex-direction: column;
	padding: 40rpx 30rpx;
}

.header {
	margin-top: 60rpx;
	margin-bottom: 40rpx;
	text-align: center;
}

.title {
	display: block;
	font-size: 72rpx;
	font-weight: 800;
	color: #1C1C1E;
	letter-spacing: 4rpx;
	margin-bottom: 16rpx;
}

.subtitle {
	display: block;
	font-size: 28rpx;
	color: #8E8E93;
	font-weight: 500;
	letter-spacing: 2rpx;
}

.content {
	flex: 1;
	display: flex;
	flex-direction: column;
	justify-content: center;
}

.intro-card {
	background-color: #FFFFFF;
	border-radius: 24rpx;
	padding: 36rpx;
	margin-bottom: 60rpx;
	box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
}

.intro-text {
	font-size: 28rpx;
	line-height: 1.8;
	color: #3A3A3C;
}

.btn-group {
	display: flex;
	flex-direction: column;
	gap: 30rpx;
}

.btn-wrapper {
	transition: transform 0.1s ease;
}

.btn-wrapper:active {
	transform: scale(0.98);
}

.btn {
	display: flex;
	align-items: center;
	justify-content: center;
	gap: 20rpx;
	padding: 40rpx;
	border-radius: 24rpx;
	box-shadow: 0 8rpx 24rpx rgba(0, 0, 0, 0.12);
	transition: all 0.1s ease;
}

.btn-icon {
	font-size: 56rpx;
}

.btn-text {
	font-size: 36rpx;
	font-weight: 700;
	color: #FFFFFF;
	letter-spacing: 2rpx;
}

.btn-compass {
	background: linear-gradient(145deg, #7C4DFF 0%, #536DFE 100%);
	border-bottom: 6rpx solid #303F9F;
}

.btn-compass:active {
	border-bottom-width: 3rpx;
	transform: translateY(3rpx);
	box-shadow: 0 4rpx 12rpx rgba(83, 109, 254, 0.3);
}

.btn-liuyao {
	background: linear-gradient(145deg, #FF6D00 0%, #FF9F0A 100%);
	border-bottom: 6rpx solid #E65100;
}

.btn-liuyao:active {
	border-bottom-width: 3rpx;
	transform: translateY(3rpx);
	box-shadow: 0 4rpx 12rpx rgba(255, 109, 0, 0.3);
}

.footer {
	text-align: center;
	padding: 40rpx 0;
}

.footer-text {
	font-size: 24rpx;
	color: #8E8E93;
	letter-spacing: 4rpx;
}
</style>