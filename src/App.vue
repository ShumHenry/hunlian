<script setup lang="ts">
import LoveForm from './components/LoveForm.vue'
</script>

<template>
  <div class="container">
    <h1>AI 恋爱契合度预测</h1>
    <el-card class="card">
      <template #header>
        <div class="card-header">
          <span>请填写信息</span>
        </div>
      </template>
      <LoveForm />
    </el-card>
  </div>
</template>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.card {
  margin-top: 20px;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
