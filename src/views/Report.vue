<!-- src/views/Report.vue -->
<template>
  <div class="container-fluid vh-100 d-flex align-items-center justify-content-center py-4">
    <ReportView 
      v-if="hasReport"
      :result="reportData"
      :loading="loading"
      :error="error"
      :errorMessage="errorMessage" 
      @retry="retryAnalysis"
      @regenerate="navigateToColorTest"
      @cancel="navigateToColorTest"
    />
    
    <div v-else class="card shadow-sm">
      <div class="card-body text-center p-5">
        <div class="display-1 mb-4">📊</div>
        <h2 class="h3 mb-3">暂无分析报告</h2>
        <p class="mb-4">请先进行个人色彩测试以生成您的专属分析报告</p>
        <button class="btn btn-primary btn-lg px-4" @click="navigateToColorTest">
          开始测试
          <i class="bi bi-arrow-right ms-2"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import ReportView from '../components/ColorTest/ReportView.vue'

const router = useRouter()
const hasReport = ref(false)
const reportData = ref('')
const loading = ref(false)
const error = ref(false)
const errorMessage = ref('')

// 从会话存储中获取报告数据
onMounted(() => {
  try {
    const savedReport = sessionStorage.getItem('colorReport')
    const savedColors = sessionStorage.getItem('colorSelection')
    
    if (savedReport) {
      reportData.value = savedReport
      hasReport.value = true
    }
  } catch (e) {
    console.error('无法获取保存的报告:', e)
  }
})

// 重试分析
const retryAnalysis = () => {
  navigateToColorTest()
}

// 导航到色彩测试页面
const navigateToColorTest = () => {
  router.push('/color-test')
}
</script>

<style scoped>
/* 移除所有自定义样式，完全使用Bootstrap类 */
</style>