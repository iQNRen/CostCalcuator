methods: {
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

// 保存到设备列表
const deviceList = uni.getStorageSync('deviceList') || []
const deviceData = {
...this.formData,
...this.costResult,
date: new Date().toLocaleString()
}
deviceList.unshift(deviceData)
uni.setStorageSync('deviceList', deviceList)

// 返回首页
uni.navigateBack()
}
}