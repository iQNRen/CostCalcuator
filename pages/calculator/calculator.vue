<template>
    <view class="container">
        <view class="form-container">
            <view class="input-group">
                <text class="label">设备名称</text>
                <input type="text" v-model="formData.name" placeholder="请输入设备名称" />
            </view>

            <view class="input-group">
                <text class="label">购买价格 (元)</text>
                <input type="digit" v-model="formData.price" placeholder="请输入购买价格" />
            </view>

            <view class="input-group">
                <text class="label">购买日期</text>
                <picker mode="date" :value="formData.purchaseDate" @change="onDateChange">
                    <view class="picker-value">{{ formData.purchaseDate || '请选择购买日期' }}</view>
                </picker>
            </view>

            <view class="input-group">
                <text class="label">预计使用时间 (月)</text>
                <input type="number" v-model="formData.lifespan" placeholder="请输入预计使用时间" />
            </view>

            <view class="input-group">
                <text class="label">每周使用天数</text>
                <input type="number" v-model="formData.daysPerWeek" placeholder="请输入每周使用天数" maxlength="1" />
            </view>

            <view class="input-group">
                <text class="label">设备图片</text>
                <view class="image-upload" @tap="chooseImage">
                    <image v-if="formData.image" :src="formData.image" mode="aspectFit" class="preview-image"></image>
                    <view v-else class="upload-placeholder">
                        <text class="upload-icon">+</text>
                        <text>点击上传图片</text>
                    </view>
                </view>
            </view>

            <button class="calculate-btn" @click="calculateCost">计算成本</button>

            <view class="result" v-if="costResult.dailyCost">
                <view class="result-item">
                    <text class="result-label">日折旧成本:</text>
                    <text class="result-value">¥{{ costResult.dailyCost }}/天</text>
                </view>
                <view class="result-item">
                    <text class="result-label">月折旧成本:</text>
                    <text class="result-value">¥{{ costResult.monthlyCost }}/月</text>
                </view>
                <view class="result-item">
                    <text class="result-label">实际使用成本:</text>
                    <text class="result-value">¥{{ costResult.actualCost }}/天</text>
                </view>
            </view>
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            formData: {
                name: '',
                price: '',
                purchaseDate: '',
                lifespan: '',
                daysPerWeek: '',
                image: ''
            },
            costResult: {
                dailyCost: null,
                monthlyCost: null,
                actualCost: null
            }
        }
    },
    methods: {
        onDateChange(e) {
            this.formData.purchaseDate = e.detail.value
        },
        chooseImage() {
            uni.chooseImage({
                count: 1,
                success: (res) => {
                    this.formData.image = res.tempFilePaths[0]
                }
            })
        },
        calculateCost() {
            if (!this.validateForm()) {
                return
            }

            const price = Number(this.formData.price)
            const lifespan = Number(this.formData.lifespan)
            const daysPerWeek = Number(this.formData.daysPerWeek)

            // 计算每天折旧成本
            const dailyCost = (price / (lifespan * 30)).toFixed(2)
            // 计算每月折旧成本
            const monthlyCost = (price / lifespan).toFixed(2)
            // 计算实际使用成本（考虑每周使用天数）
            const actualCost = ((price / (lifespan * 30)) * (7 / daysPerWeek)).toFixed(2)

            this.costResult = {
                dailyCost,
                monthlyCost,
                actualCost
            }

            // 保存到历史记录
            const history = uni.getStorageSync('costHistory') || []
            history.unshift({
                ...this.formData,
                ...this.costResult,
                date: new Date().toLocaleString()
            })
            uni.setStorageSync('costHistory', history)
        },
        validateForm() {
            if (!this.formData.name) {
                uni.showToast({
                    title: '请输入设备名称',
                    icon: 'none'
                })
                return false
            }
            if (!this.formData.price) {
                uni.showToast({
                    title: '请输入购买价格',
                    icon: 'none'
                })
                return false
            }
            if (!this.formData.purchaseDate) {
                uni.showToast({
                    title: '请选择购买日期',
                    icon: 'none'
                })
                return false
            }
            if (!this.formData.lifespan) {
                uni.showToast({
                    title: '请输入预计使用时间',
                    icon: 'none'
                })
                return false
            }
            if (!this.formData.daysPerWeek || this.formData.daysPerWeek > 7) {
                uni.showToast({
                    title: '请输入正确的每周使用天数',
                    icon: 'none'
                })
                return false
            }
            return true
        }
    }
}
</script>

<style>
.container {
    padding: 20px;
}

.form-container {
    background: #fff;
    padding: 15px;
    border-radius: 8px;
}

.input-group {
    margin-bottom: 15px;
}

.label {
    display: block;
    margin-bottom: 5px;
    color: #333;
}

input,
.picker-value {
    width: 100%;
    height: 40px;
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 0 10px;
    line-height: 40px;
}

.picker-value {
    color: #666;
}

.image-upload {
    width: 100%;
    height: 200px;
    border: 1px dashed #ddd;
    border-radius: 4px;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
}

.upload-placeholder {
    text-align: center;
    color: #999;
}

.upload-icon {
    font-size: 40px;
    display: block;
}

.preview-image {
    width: 100%;
    height: 100%;
}

.calculate-btn {
    background: #3cc51f;
    color: #fff;
    margin: 20px 0;
}

.result {
    margin-top: 20px;
    padding: 15px;
    background: #f8f8f8;
    border-radius: 4px;
}

.result-item {
    display: flex;
    justify-content: space-between;
    margin-bottom: 10px;
}

.result-label {
    color: #666;
}

.result-value {
    color: #3cc51f;
    font-weight: bold;
}
</style>