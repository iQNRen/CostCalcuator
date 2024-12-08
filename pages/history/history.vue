<template>
    <view class="container">
        <view class="history-list">
            <view class="history-item" v-for="(item, index) in historyList" :key="index">
                <view class="history-header">
                    <text class="date">{{ item.date }}</text>
                </view>

                <view class="device-info">
                    <image v-if="item.image" :src="item.image" mode="aspectFit" class="device-image"></image>
                    <view class="device-details">
                        <text class="device-name">{{ item.name }}</text>
                        <text class="device-price">购买价格: ¥{{ item.price }}</text>
                        <text class="device-date">购买日期: {{ item.purchaseDate }}</text>
                    </view>
                </view>

                <view class="cost-details">
                    <view class="cost-item">
                        <text class="cost-label">日折旧成本:</text>
                        <text class="cost-value">¥{{ item.dailyCost }}/天</text>
                    </view>
                    <view class="cost-item">
                        <text class="cost-label">月折旧成本:</text>
                        <text class="cost-value">¥{{ item.monthlyCost }}/月</text>
                    </view>
                    <view class="cost-item">
                        <text class="cost-label">实际使用成本:</text>
                        <text class="cost-value">¥{{ item.actualCost }}/天</text>
                    </view>
                    <view class="usage-info">
                        <text>预计使用: {{ item.lifespan }}个月</text>
                        <text>每周使用: {{ item.daysPerWeek }}天</text>
                    </view>
                </view>
            </view>

            <view class="empty-state" v-if="historyList.length === 0">
                <text>暂无计算历史</text>
            </view>
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            historyList: []
        }
    },
    onShow() {
        this.historyList = uni.getStorageSync('costHistory') || []
    }
}
</script>

<style>
.container {
    padding: 20px;
}

.history-item {
    background: #fff;
    border-radius: 8px;
    padding: 15px;
    margin-bottom: 15px;
}

.history-header {
    margin-bottom: 10px;
}

.date {
    color: #666;
    font-size: 14px;
}

.device-info {
    display: flex;
    margin-bottom: 15px;
    padding-bottom: 15px;
    border-bottom: 1px solid #eee;
}

.device-image {
    width: 100px;
    height: 100px;
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

.cost-details {
    background: #f8f8f8;
    padding: 10px;
    border-radius: 4px;
}

.cost-item {
    display: flex;
    justify-content: space-between;
    margin-bottom: 5px;
}

.cost-label {
    color: #666;
}

.cost-value {
    color: #3cc51f;
    font-weight: bold;
}

.usage-info {
    margin-top: 10px;
    padding-top: 10px;
    border-top: 1px solid #eee;
    display: flex;
    justify-content: space-between;
    color: #666;
    font-size: 14px;
}

.empty-state {
    text-align: center;
    padding: 40px 0;
    color: #999;
}
</style>