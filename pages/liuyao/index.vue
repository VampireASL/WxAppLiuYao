<template>
	<view class="container">
		<view class="header">
			<view class="back-btn" @click="goBack">
				<text class="back-icon">←</text>
			</view>
			<text class="title">天道</text>
			<view class="placeholder"></view>
		</view>

		<view class="disclaimer">
			<text class="disclaimer-text">仅供学习无不良引导</text>
			<text class="disclaimer-text">内容仅供娱乐切勿相信封建迷信</text>
			<text class="disclaimer-text">相信科学</text>
		</view>

		<scroll-view class="scroll-container" scroll-y>
			<view class="tab-container">
				<view 
				class="tab-item" 
				:class="{ active: currentTab === 'time' }" 
				@click="currentTab = 'time'"
			>
				时间探索
			</view>
				<view 
					class="tab-item" 
					:class="{ active: currentTab === 'random' }" 
					@click="currentTab = 'random'"
				>
					随机数字
				</view>
			</view>

			<view class="hand-container">
				<view class="hand">
					<view class="finger-row top">
						<view 
							class="palace-item" 
							:class="{ active: activePalace === 1 }"
						>
							<text class="palace-name">留连</text>
							<text class="palace-num">2</text>
						</view>
						<view 
							class="palace-item" 
							:class="{ active: activePalace === 2 }"
						>
							<text class="palace-name">速喜</text>
							<text class="palace-num">3</text>
						</view>
						<view 
							class="palace-item" 
							:class="{ active: activePalace === 3 }"
						>
							<text class="palace-name">赤口</text>
							<text class="palace-num">4</text>
						</view>
					</view>
					<view class="finger-row bottom">
						<view 
							class="palace-item" 
							:class="{ active: activePalace === 0 }"
						>
							<text class="palace-name">大安</text>
							<text class="palace-num">1</text>
						</view>
						<view 
							class="palace-item" 
							:class="{ active: activePalace === 5 }"
						>
							<text class="palace-name">空亡</text>
							<text class="palace-num">6</text>
						</view>
						<view 
							class="palace-item" 
							:class="{ active: activePalace === 4 }"
						>
							<text class="palace-name">小吉</text>
							<text class="palace-num">5</text>
						</view>
					</view>
				</view>
			</view>

			<view v-if="currentTab === 'time'" class="form-container">
				<view class="form-item">
					<text class="label">月份</text>
					<input 
						class="input" 
						v-model="lunarMonth" 
						type="number" 
						placeholder="请输入月份(1-12)"
					/>
				</view>

				<view class="form-item">
					<text class="label">日期</text>
					<input 
						class="input" 
						v-model="lunarDay" 
						type="number" 
						placeholder="请输入日期(1-30)"
					/>
				</view>

				<view class="form-item">
					<text class="label">时辰</text>
					<picker mode="selector" :range="shichenList" :value="shichenIndex" @change="onShichenChange">
						<view class="picker">{{ selectedShichen }}</view>
					</picker>
				</view>

				<button class="btn btn-3d" @click="calculateByTime">开始探索</button>
			</view>

			<view v-if="currentTab === 'random'" class="form-container">
				<view class="form-item">
					<text class="label">随机数字</text>
					<input 
						class="input" 
						v-model="randomNumber" 
						type="number" 
						placeholder="请输入随机数字"
					/>
				</view>

				<button class="btn btn-3d" @click="calculateByRandom">开始探索</button>
			</view>

			<view v-if="result" class="result-container">
				<view class="result-title">探索结果</view>
				<view class="result-palace" :style="{ color: palaceColors[result.palaceIndex] }">
					{{ result.name }}
				</view>
				<view class="result-level" :class="result.levelClass">
					{{ result.level }}
				</view>
				<view class="result-detail">
					<view class="detail-item">
						<text class="detail-label">五行</text>
						<text class="detail-value">{{ result.wuxing }}</text>
					</view>
					<view class="detail-item">
						<text class="detail-label">颜色</text>
						<text class="detail-value">{{ result.yanse }}</text>
					</view>
					<view class="detail-item">
						<text class="detail-label">方位</text>
						<text class="detail-value">{{ result.fangwei }}</text>
					</view>
					<view class="detail-item">
						<text class="detail-label">神煞</text>
						<text class="detail-value">{{ result.shensha }}</text>
					</view>
					<view class="detail-item full">
						<text class="detail-label">主数</text>
						<text class="detail-value">{{ result.zhushu }}</text>
					</view>
				</view>
				<view class="result-section">
					<text class="section-title">意象</text>
					<text class="section-text">{{ result.yixiang }}</text>
				</view>
				<view class="result-section">
					<text class="section-title">详细解读</text>
					<view class="interpretation-list">
						<view v-for="(item, index) in result.interpretations" :key="index" class="interpret-item">
							<text class="interpret-label">{{ item.title }}：</text>
							<text class="interpret-text">{{ item.content }}</text>
						</view>
					</view>
				</view>
				<view class="result-jue">
					<text class="jue-label">歌诀</text>
					<text class="jue-text">{{ result.jue }}</text>
				</view>
			</view>

			<view class="palace-list">
				<view class="section-header">
					<text class="list-title">六宫详解</text>
				</view>
				<view v-for="(palace, index) in palaceList" :key="index" class="palace-card card-3d">
					<view class="card-header">
						<view class="card-title" :style="{ color: palaceColors[index] }">
							{{ palace.name }}
						</view>
						<view class="card-level" :class="palace.levelClass">
							{{ palace.level }}
						</view>
					</view>
					<view class="card-content">
						<view class="card-line">
							<text class="card-label">意象：</text>{{ palace.yixiang }}
						</view>
						<view class="card-line">
							<text class="card-label">五行：</text>{{ palace.wuxing }}
						</view>
						<view class="card-line">
							<text class="card-label">颜色：</text>{{ palace.yanse }}
						</view>
						<view class="card-line">
							<text class="card-label">方位：</text>{{ palace.fangwei }}
						</view>
						<view class="card-line">
							<text class="card-label">神煞：</text>{{ palace.shensha }}
						</view>
						<view class="card-line">
							<text class="card-label">歌诀：</text>{{ palace.jue }}
						</view>
					</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
const PALACE_LIST = [
	{
		name: '大安',
		level: '大吉',
		levelClass: 'level-great',
		yixiang: '安定、平稳、顺利、不动',
		wuxing: '木',
		yanse: '青色',
		fangwei: '东方',
		shensha: '青龙',
		zhushu: '1、5、7',
		jue: '大安事事昌，求谋在东方。失物不远去，宅舍保安康。',
		interpretations: [
			{ title: '事业', content: '工作稳定，宜守不宜攻，适合按部就班。' },
			{ title: '感情', content: '关系和谐，无大波澜，单身者缘分未到。' },
			{ title: '财运', content: '收支平衡，不宜投机。' },
			{ title: '健康', content: '身体康健，小病易愈。' },
			{ title: '失物', content: '在家或常去之处，易找回。' },
			{ title: '出行', content: '平安，无大碍。' }
		]
	},
	{
		name: '留连',
		level: '小凶',
		levelClass: 'level-small-bad',
		yixiang: '迟缓、纠缠、拖延、反复',
		wuxing: '水',
		yanse: '黑色',
		fangwei: '南方',
		shensha: '玄武',
		zhushu: '2、8、10',
		jue: '留连事难成，求谋日不明。官司宜谨慎，去者未回程。',
		interpretations: [
			{ title: '事业', content: '事情进展缓慢，有阻碍，需耐心等待。' },
			{ title: '感情', content: '纠缠不清，有口舌或冷战，需主动沟通。' },
			{ title: '财运', content: '资金回笼慢，不宜投资。' },
			{ title: '健康', content: '慢性病或久病难愈。' },
			{ title: '失物', content: '难找回，或被他人收管。' },
			{ title: '出行', content: '有延误，宜改期。' }
		]
	},
	{
		name: '速喜',
		level: '中吉',
		levelClass: 'level-mid',
		yixiang: '快速、喜事、消息、圆满',
		wuxing: '火',
		yanse: '红色',
		fangwei: '南方',
		shensha: '朱雀',
		zhushu: '3、6、9',
		jue: '速喜喜来临，求财向南行。失物申午见，行人路上寻。',
		interpretations: [
			{ title: '事业', content: '事情进展快，有贵人相助，宜主动出击。' },
			{ title: '感情', content: '迅速升温，有喜讯(如表白成功)。' },
			{ title: '财运', content: '有意外之财或快速回报。' },
			{ title: '健康', content: '迅速康复。' },
			{ title: '失物', content: '可找回，在南方或火性场所。' },
			{ title: '出行', content: '顺利，甚至提前到达。' }
		]
	},
	{
		name: '赤口',
		level: '大凶',
		levelClass: 'level-big-bad',
		yixiang: '口舌、是非、破败、官非',
		wuxing: '金',
		yanse: '白色',
		fangwei: '西方',
		shensha: '白虎',
		zhushu: '4、7、10',
		jue: '赤口主口舌，官非切要防。失物急去寻，行人有惊慌。',
		interpretations: [
			{ title: '事业', content: '易与人争执，有小人，合同纠纷。' },
			{ title: '感情', content: '争吵、分手之象，说话需谨慎。' },
			{ title: '财运', content: '破财、损失、不宜投资。' },
			{ title: '健康', content: '意外伤、手术、急症。' },
			{ title: '失物', content: '难找回，或被他人破坏。' },
			{ title: '出行', content: '不利，易遇麻烦。' }
		]
	},
	{
		name: '小吉',
		level: '上吉',
		levelClass: 'level-top',
		yixiang: '人和、喜事、合作、顺利',
		wuxing: '木',
		yanse: '绿色',
		fangwei: '东方/东北',
		shensha: '六合',
		zhushu: '5、8、11',
		jue: '小吉最吉昌，路上好商量。阳人来报喜，失物在坤方。',
		interpretations: [
			{ title: '事业', content: '合作顺利，得人相助，宜团队协作。' },
			{ title: '感情', content: '良缘将至，人际关系和谐。' },
			{ title: '财运', content: '稳定增长，有小财。' },
			{ title: '健康', content: '身体康健，无大碍。' },
			{ title: '失物', content: '可找回，在草木附近。' },
			{ title: '出行', content: '顺遂，且有人接待。' }
		]
	},
	{
		name: '空亡',
		level: '大凶',
		levelClass: 'level-big-bad',
		yixiang: '落空、无成、忧虑、虚无',
		wuxing: '土',
		yanse: '黄色',
		fangwei: '中央/西南',
		shensha: '勾陈',
		zhushu: '6、9、12',
		jue: '空亡事不长，阴人多乖张。求财无利益，行人有灾殃。',
		interpretations: [
			{ title: '事业', content: '事情难成，努力白费，宜暂停观望。' },
			{ title: '感情', content: '单相思、缘分未到、分离。' },
			{ title: '财运', content: '亏损、被骗、竹篮打水。' },
			{ title: '健康', content: '久病难愈，精神不振。' },
			{ title: '失物', content: '难以找回，如同蒸发。' },
			{ title: '出行', content: '白跑一趟，无功而返。' }
		]
	}
];

export default {
	data() {
		return {
			currentTab: 'time',
			activePalace: -1,
			lunarMonth: 4,
			lunarDay: 1,
			shichenList: [
				'子时(23-1)', '丑时(1-3)', '寅时(3-5)', '卯时(5-7)',
				'辰时(7-9)', '巳时(9-11)', '午时(11-13)', '未时(13-15)',
				'申时(15-17)', '酉时(17-19)', '戌时(19-21)', '亥时(21-23)'
			],
			shichenIndex: 0,
			selectedShichen: '子时(23-1)',
			randomNumber: '',
			result: null,
			palaceList: PALACE_LIST,
			palaceColors: ['#006400', '#00008B', '#DC143C', '#C0C0C0', '#228B22', '#DAA520']
		}
	},
	onLoad() {
		this.initSimple();
	},
	methods: {
		goBack() {
			uni.navigateBack();
		},

		initSimple() {
			const now = new Date();
			const hour = now.getHours();
			this.shichenIndex = this.getShichenIndex(hour);
			this.selectedShichen = this.shichenList[this.shichenIndex];
		},

		getShichenIndex(hour) {
			if (hour >= 23 || hour < 1) return 0;
			if (hour >= 1 && hour < 3) return 1;
			if (hour >= 3 && hour < 5) return 2;
			if (hour >= 5 && hour < 7) return 3;
			if (hour >= 7 && hour < 9) return 4;
			if (hour >= 9 && hour < 11) return 5;
			if (hour >= 11 && hour < 13) return 6;
			if (hour >= 13 && hour < 15) return 7;
			if (hour >= 15 && hour < 17) return 8;
			if (hour >= 17 && hour < 19) return 9;
			if (hour >= 19 && hour < 21) return 10;
			return 11;
		},

		onShichenChange(e) {
			this.shichenIndex = e.detail.value;
			this.selectedShichen = this.shichenList[this.shichenIndex];
		},

		calculatePalace(startIndex, count) {
			return (startIndex + count - 1) % 6;
		},

		calculateByTime() {
			const month = parseInt(this.lunarMonth) || 1;
			const day = parseInt(this.lunarDay) || 1;
			const shichen = this.shichenIndex + 1;

			if (month < 1 || month > 12) {
				uni.showToast({
					title: '请输入有效的月份(1-12)',
					icon: 'none'
				});
				return;
			}

			if (day < 1 || day > 30) {
				uni.showToast({
					title: '请输入有效的日期(1-30)',
					icon: 'none'
				});
				return;
			}

			let currentIndex = 0;
			currentIndex = this.calculatePalace(currentIndex, month);
			currentIndex = this.calculatePalace(currentIndex, day);
			currentIndex = this.calculatePalace(currentIndex, shichen);

			this.showResult(currentIndex);
		},

		calculateByRandom() {
			if (!this.randomNumber) {
				uni.showToast({
					title: '请输入随机数字',
					icon: 'none'
				});
				return;
			}

			const num = parseInt(this.randomNumber) || 1;
			let currentIndex = this.calculatePalace(0, num);
			this.showResult(currentIndex);
		},

		showResult(palaceIndex) {
			if (palaceIndex < 0 || palaceIndex >= this.palaceList.length) {
				palaceIndex = 0;
			}
			
			const palace = this.palaceList[palaceIndex];
			this.result = {
				...palace,
				palaceIndex: palaceIndex
			};
			this.activePalace = palaceIndex;
		}
	}
};
</script>

<style scoped>
.container {
	min-height: 100vh;
	background: linear-gradient(180deg, #F2F2F7 0%, #E5E5EA 100%);
	display: flex;
	flex-direction: column;
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

.disclaimer {
	background-color: #FFF3E0;
	border-radius: 16rpx;
	padding: 24rpx 30rpx;
	margin: 20rpx 20rpx 0 20rpx;
	border-left: 6rpx solid #FF9500;
	box-shadow: 0 2rpx 8rpx rgba(255, 149, 0, 0.15);
}

.disclaimer-text {
	display: block;
	font-size: 26rpx;
	color: #BF360C;
	font-weight: 600;
	text-align: center;
	line-height: 1.8;
}

.scroll-container {
	flex: 1;
	height: 0;
	padding: 20rpx 30rpx 60rpx;
}

.tab-container {
	display: flex;
	background-color: #E5E5EA;
	border-radius: 16rpx;
	margin-bottom: 30rpx;
	padding: 8rpx;
	box-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.06);
}

.tab-item {
	flex: 1;
	text-align: center;
	padding: 24rpx 20rpx;
	border-radius: 12rpx;
	font-size: 30rpx;
	font-weight: 600;
	color: #8E8E93;
	transition: all 0.3s ease;
}

.tab-item.active {
	background-color: #FFFFFF;
	color: #007AFF;
	box-shadow: 0 2rpx 12rpx rgba(0, 122, 255, 0.15);
}

.hand-container {
	display: flex;
	justify-content: center;
	margin-bottom: 40rpx;
}

.hand {
	width: 620rpx;
	height: 320rpx;
}

.finger-row {
	display: flex;
	justify-content: space-around;
	margin-bottom: 30rpx;
}

.palace-item {
	width: 170rpx;
	height: 130rpx;
	background: linear-gradient(145deg, #FFFFFF 0%, #F5F5F7 100%);
	border: 2rpx solid #D1D1D6;
	border-radius: 20rpx;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	transition: all 0.3s ease;
	box-shadow: 0 4rpx 12rpx rgba(0, 0, 0, 0.08);
}

.palace-item.active {
	background: linear-gradient(145deg, #FFD60A 0%, #FFCC00 100%);
	border-color: #FF9F0A;
	transform: scale(1.12);
	box-shadow: 0 8rpx 24rpx rgba(255, 204, 0, 0.4);
}

.palace-name {
	font-size: 32rpx;
	font-weight: 700;
	color: #1C1C1E;
}

.palace-num {
	font-size: 24rpx;
	color: #8E8E93;
	margin-top: 6rpx;
	font-weight: 600;
}

.form-container {
	background-color: #FFFFFF;
	border-radius: 24rpx;
	padding: 40rpx 30rpx;
	margin-bottom: 30rpx;
	box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.06);
}

.form-item {
	margin-bottom: 36rpx;
}

.label {
	display: block;
	font-size: 28rpx;
	font-weight: 600;
	color: #1C1C1E;
	margin-bottom: 16rpx;
	letter-spacing: 0.3px;
}

.picker {
	background-color: #F2F2F7;
	border-radius: 14rpx;
	padding: 28rpx 24rpx;
	font-size: 30rpx;
	color: #1C1C1E;
	font-weight: 500;
	border: 1px solid transparent;
	transition: all 0.2s ease;
}

.picker:active {
	background-color: #E5E5EA;
}

.input {
	background-color: #F2F2F7;
	border-radius: 14rpx;
	padding: 28rpx 24rpx;
	font-size: 30rpx;
	color: #1C1C1E;
	font-weight: 500;
	border: 1px solid transparent;
	transition: all 0.2s ease;
	width: 100%;
}

.input:focus {
	background-color: #FFFFFF;
	border-color: #007AFF;
	box-shadow: 0 0 0 4rpx rgba(0, 122, 255, 0.1);
}

.btn {
	width: 100%;
	background-color: #007AFF;
	color: #FFFFFF;
	border-radius: 16rpx;
	font-size: 36rpx;
	font-weight: 700;
	padding: 36rpx;
	margin-top: 10rpx;
	border: none;
	display: flex;
	align-items: center;
	justify-content: center;
	letter-spacing: 0.5px;
}

.btn-3d {
	background: linear-gradient(180deg, #0080FF 0%, #007AFF 50%, #0066CC 100%);
	box-shadow: 0 8rpx 24rpx rgba(0, 122, 255, 0.35);
	border-bottom: 6rpx solid #0055AA;
	position: relative;
	transition: all 0.1s ease;
}

.btn-3d:active {
	transform: translateY(3rpx);
	box-shadow: 0 4rpx 12rpx rgba(0, 122, 255, 0.3);
	border-bottom-width: 3rpx;
}

.result-container {
	background: linear-gradient(145deg, #FFFFFF 0%, #F9F9FB 100%);
	border-radius: 28rpx;
	padding: 40rpx 32rpx;
	margin-bottom: 30rpx;
	box-shadow: 0 6rpx 24rpx rgba(0, 0, 0, 0.08);
	border: 2rpx solid #FFD60A;
}

.result-title {
	font-size: 32rpx;
	font-weight: 700;
	text-align: center;
	color: #1C1C1E;
	margin-bottom: 24rpx;
	letter-spacing: 0.5px;
}

.result-palace {
	font-size: 80rpx;
	font-weight: 800;
	text-align: center;
	margin: 20rpx 0;
	letter-spacing: 4rpx;
	text-shadow: 0 2rpx 8rpx rgba(0, 0, 0, 0.1);
}

.result-level {
	text-align: center;
	font-size: 28rpx;
	font-weight: 700;
	padding: 10rpx 30rpx;
	border-radius: 30rpx;
	margin: 0 auto 30rpx;
	width: fit-content;
}

.level-great {
	background-color: #E8F5E9;
	color: #2E7D32;
}

.level-top {
	background-color: #E3F2FD;
	color: #1565C0;
}

.level-mid {
	background-color: #FFF3E0;
	color: #E65100;
}

.level-small-bad {
	background-color: #FFF3E0;
	color: #BF360C;
}

.level-big-bad {
	background-color: #FFEBEE;
	color: #C62828;
}

.result-detail {
	display: flex;
	flex-wrap: wrap;
	gap: 20rpx;
	margin: 30rpx 0;
}

.detail-item {
	flex: 1;
	min-width: 45%;
	background-color: #F2F2F7;
	padding: 20rpx 24rpx;
	border-radius: 14rpx;
	display: flex;
	flex-direction: column;
	align-items: center;
}

.detail-item.full {
	flex: 100%;
	min-width: 100%;
}

.detail-label {
	font-weight: 700;
	color: #8E8E93;
	font-size: 24rpx;
	margin-bottom: 6rpx;
}

.detail-value {
	font-size: 28rpx;
	font-weight: 600;
	color: #1C1C1E;
}

.result-section {
	margin-top: 30rpx;
}

.section-title {
	display: block;
	font-size: 28rpx;
	font-weight: 700;
	color: #FF9F0A;
	margin-bottom: 12rpx;
}

.section-text {
	display: block;
	font-size: 28rpx;
	line-height: 1.8;
	color: #3A3A3C;
	background-color: #F2F2F7;
	padding: 20rpx;
	border-radius: 12rpx;
}

.interpretation-list {
	background-color: #F2F2F7;
	border-radius: 12rpx;
	padding: 20rpx;
}

.interpret-item {
	margin-bottom: 16rpx;
	line-height: 1.8;
}

.interpret-item:last-child {
	margin-bottom: 0;
}

.interpret-label {
	font-weight: 700;
	color: #007AFF;
	font-size: 28rpx;
}

.interpret-text {
	font-size: 28rpx;
	color: #3A3A3C;
}

.result-jue {
	margin-top: 30rpx;
}

.jue-label {
	display: block;
	font-size: 28rpx;
	font-weight: 700;
	color: #FF9F0A;
	margin-bottom: 12rpx;
}

.jue-text {
	display: block;
	font-size: 28rpx;
	line-height: 1.8;
	color: #3A3A3C;
	font-weight: 500;
	font-style: italic;
	background-color: #FFF3E0;
	padding: 24rpx;
	border-radius: 16rpx;
	border-left: 6rpx solid #FF9F0A;
}

.section-header {
	margin-bottom: 24rpx;
	text-align: center;
}

.list-title {
	font-size: 36rpx;
	font-weight: 700;
	color: #1C1C1E;
	letter-spacing: 0.5px;
}

.palace-card {
	background: linear-gradient(145deg, #FFFFFF 0%, #F8F8FA 100%);
	border-radius: 20rpx;
	padding: 32rpx 28rpx;
	margin-bottom: 24rpx;
	border: 1px solid #E5E5EA;
}

.card-3d {
	box-shadow: 0 4rpx 16rpx rgba(0, 0, 0, 0.06);
	transition: all 0.3s ease;
}

.card-header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-bottom: 20rpx;
}

.card-title {
	font-size: 36rpx;
	font-weight: 800;
	letter-spacing: 1px;
}

.card-level {
	font-size: 24rpx;
	font-weight: 700;
	padding: 8rpx 20rpx;
	border-radius: 20rpx;
}

.card-content {
	font-size: 28rpx;
	color: #3A3A3C;
	line-height: 2;
}

.card-line {
	margin-bottom: 12rpx;
}

.card-label {
	font-weight: 700;
	color: #8E8E93;
}
</style>
