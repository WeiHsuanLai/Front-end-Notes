## 1. SETUP() 階段
#### 組合式 API 的起點

取代了 Vue 2 的 `beforeCreate` 與 `created`。  
**注意：** 此時組件實例尚未掛載到 DOM。

### 2. 掛載階段 (onMounted)

#### 資源初始化的黃金時機
**用法：** 這是最常用的掛鉤。用於發送 API 請求、操作 DOM 元素或設定計時器

### 3. 更新階段 (onUpdated)
### 畫面渲染後的副作用
**用法：** 在數據更新導致 DOM 重新渲染後執行。
💡 **典型應用：即時聊天室。**  
當訊息清單更新後，我們需要立刻將捲軸「拉到底部」。因為清單的高度是在渲染後才改變的，所以必須在 `onUpdated` 中操作。

### 4. 卸載階段 (onUnmounted)

#### 資源清理與預防溢漏
用於清除定時器、中斷 API 請求或移除全域事件監聽，確保組件消失後不佔效能。

```
<template>
  <div class="product-card">
    <h2>{{ loading ? '載入中...' : product.name }}</h2>
    <p>目前價格：${{ product.price }}</p>
    <p>頁面已停留：{{ timerCount }} 秒</p>
    
    <canvas ref="chartRef" width="200" height="100"></canvas>
    
    <button @click="product.price += 10">手動漲價</button>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue'

// 響應式數據
const product = ref({ name: '', price: 0 })
const loading = ref(true)
const timerCount = ref(0)
const chartRef = ref(null) // 用於操作 DOM
let timer = null

// --- 1. Setup (代替 created) ---
// 適合：初始化不涉及 DOM 的數據，或定義變數
console.log('Setup: 初始化數據')


// --- 2. onMounted ---
// 適合：發送 API 請求、操作真實 DOM、啟動計時器、綁定全局事件
onMounted(async () => {
  console.log('onMounted: 組件已掛載')
  
  // 模擬 API 請求
  setTimeout(() => {
    product.value = { name: '高階開發者鍵盤', price: 2999 }
    loading.value = false
  }, 1000)

  // 操作 DOM：初始化畫布（例如圖表庫）
  console.log('操作 DOM 元素:', chartRef.value)

  // 啟動計時器
  timer = setInterval(() => {
    timerCount.value++
  }, 1000)

  // 綁定視窗監聽
  window.addEventListener('resize', handleResize)
})


// --- 3. onBeforeUpdate & onUpdated ---
// 適合：在數據更新前後做紀錄、或根據更新後的 DOM 重新計算位置
onBeforeUpdate(() => {
  console.log('onBeforeUpdate: 價格即將變動，當前價格：', product.value.price)
})

onUpdated(() => {
  // 警告：除非必要，否則避免在這裡修改響應式數據，會導致無限循環
  console.log('onUpdated: 畫面已更新完成')
})


// --- 4. onBeforeUnmount & onUnmounted ---
// 適合：清理工作（非常重要！防止記憶體洩漏）
onBeforeUnmount(() => {
  console.log('onBeforeUnmount: 準備清理...')
})

onUnmounted(() => {
  console.log('onUnmounted: 清理完成')
  
  // 1. 清除計時器
  clearInterval(timer)
  
  // 2. 移除全局事件監聽
  window.removeEventListener('resize', handleResize)
})

// 輔助函式
const handleResize = () => {
  console.log('視窗大小改變中...')
}
</script>

<style scoped></style>
```
