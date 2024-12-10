<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps<{
  visible: boolean
  result?: {
    score: number
    analysis: string
    suggestions: string[]
  }
}>()

const emit = defineEmits<{
  (e: 'close'): void
  (e: 'update:visible', value: boolean): void
}>()

const handleClose = () => {
  emit('close')
  emit('update:visible', false)
}
</script>

<template>
  <el-dialog
    :model-value="visible"
    @update:model-value="(val) => emit('update:visible', val)"
    title="预测结果"
    width="60%"
    :close-on-click-modal="false"
    @close="handleClose"
  >
    <div v-if="result" class="result-content">
      <div class="score-section">
        <div class="score-title">契合度评分</div>
        <el-progress
          type="dashboard"
          :percentage="result.score"
          :color="customColors"
        />
      </div>
      <div class="analysis-section">
        <h4>分析报告</h4>
        <p>{{ result.analysis }}</p>
      </div>
      <div class="suggestions-section">
        <h4>建议</h4>
        <ul>
          <li v-for="(item, index) in result.suggestions" :key="index">
            {{ item }}
          </li>
        </ul>
      </div>
    </div>
  </el-dialog>
</template>

<style scoped>
.result-content {
  padding: 20px;
}

.score-section {
  text-align: center;
  margin-bottom: 30px;
}

.score-title {
  font-size: 18px;
  margin-bottom: 15px;
  color: #409EFF;
}

.analysis-section,
.suggestions-section {
  margin-top: 20px;
}

h4 {
  color: #409EFF;
  margin-bottom: 10px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin: 10px 0;
  padding-left: 20px;
  position: relative;
}

li::before {
  content: "•";
  color: #409EFF;
  position: absolute;
  left: 0;
}

:deep(.el-progress) {
  margin: 0 auto;
}
</style>

<script lang="ts">
const customColors = [
  { color: '#F56C6C', percentage: 20 },
  { color: '#E6A23C', percentage: 40 },
  { color: '#5CB87A', percentage: 60 },
  { color: '#1989FA', percentage: 80 },
  { color: '#6F7AD3', percentage: 100 }
]
</script> 