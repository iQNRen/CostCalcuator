<template>
	<view class="container">
		<view class="device-list">
			<view class="device-item" v-for="(item, index) in deviceList" :key="index">
				<view class="device-info">
					<image v-if="item.image" :src="item.image" mode="aspectFit" class="device-image"></image>
					<view class="device-details">
						<text class="device-name">{{ item.name }}</text>
						<text class="device-price">购买价格: ¥{{ item.price }}</text>
						<text class="device-date">购买日期: {{ item.purchaseDate }}</text>
					</view>
				</view>
				<view class="cost-info">
					<view class="cost-item">
						<text class="cost-label">日折旧成本</text>
						<text class="cost-value">¥{{ item.dailyCost }}/天</text>
					</view>
					<view class="cost-item">
						<text class="cost-label">实际使用成本</text>
						<text class="cost-value">¥{{ item.actualCost }}/天</text>
					</view>
				</view>
			</view>

			<view class="empty-state" v-if="deviceList.length === 0">
				<text>暂无设备，点击右下角添加</text>
			</view>
		</view>

		<!-- 添加按钮 -->
		<view class="add-btn" @tap="goToAdd">
			<text class="add-icon">+</text>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			deviceList: []
		}
	},
	onShow() {
		// 每次显示页面时重新获取设备列表
		this.deviceList = uni.getStorageSync('deviceList') || []
	},
	methods: {
		goToAdd() {
			uni.navigateTo({
				url: '/pages/add/add'
			})
		}
	}
}
</script>

<style>
.container {
	padding: 20px;
	padding-bottom: 100px;
	/* 为悬浮按钮留出空间 */
}

.device-item {
	background: #fff;
	border-radius: 8px;
	padding: 15px;
	margin-bottom: 15px;
}

.device-info {
	display: flex;
	margin-bottom: 15px;
	padding-bottom: 15px;
	border-bottom: 1px solid #eee;
}

.device-image {
	width: 80px;
	height: 80px;
	border-radius: 4px;
	margin-right: 15px;
}

.device-details {
	flex: 1;
}

.device-name {
	font-size: 18px;
	font-weight: bold;
	margin-bottom: 5px;
	display: block;
}

.device-price,
.device-date {
	font-size: 14px;
	color: #666;
	display: block;
	margin-bottom: 5px;
}

.cost-info {
	display: flex;
	justify-content: space-between;
}

.cost-item {
	text-align: center;
}

.cost-label {
	font-size: 12px;
	color: #666;
	display: block;
	margin-bottom: 5px;
}

.cost-value {
	color: #3cc51f;
	font-weight: bold;
}

.empty-state {
	text-align: center;
	padding: 40px 0;
	color: #999;
}

.add-btn {
	position: fixed;
	right: 30px;
	bottom: 30px;
	width: 60px;
	height: 60px;
	background: #3cc51f;
	border-radius: 30px;
	display: flex;
	justify-content: center;
	align-items: center;
	box-shadow: 0 2px 8px rgba(60, 197, 31, 0.3);
}

.add-icon {
	color: #fff;
	font-size: 30px;
	font-weight: bold;
}
</style>
