<script setup lang="ts">
import { ref } from 'vue'
import type { FormInstance } from 'element-plus'
import PredictResult from './PredictResult.vue'

const formRef = ref<FormInstance>()
const formData = ref({
  name1: '',
  birthday1: '',
  gender1: '',
  name2: '',
  birthday2: '',
  gender2: ''
})

const dialogVisible = ref(false)
const predictResult = ref({
  score: 0,
  analysis: '',
  suggestions: [] as string[]
})

const rules = {
  name1: [{ required: true, message: '请输入姓名', trigger: 'blur' }],
  birthday1: [{ required: true, message: '请选择日期', trigger: 'change' }],
  gender1: [{ required: true, message: '请选择性别', trigger: 'change' }],
  name2: [{ required: true, message: '请输入姓名', trigger: 'blur' }],
  birthday2: [{ required: true, message: '请选择日期', trigger: 'change' }],
  gender2: [{ required: true, message: '请选择性别', trigger: 'change' }]
}

const calculatePrediction = (data: typeof formData.value) => {
  // 这里是示例的预测逻辑，实际项目中可以调用API
  const score = Math.floor(Math.random() * 40) + 60 // 60-100的随机数
  const analysis = `基于${data.name1}和${data.name2}的生日和性别分析，你们的契合度相当不错！在感情方面有很好的默契度，能够相互理解和支持。`
  const suggestions = [
    '建议多创造共同的兴趣爱好，增进感情',
    '注意倾听对方的想法和感受',
    '保持良好的沟通习惯',
    '适时给予对方惊喜和关心'
  ]
  
  return {
    score,
    analysis,
    suggestions
  }
}

const submitForm = async (formEl: FormInstance | undefined) => {
  if (!formEl) return
  await formEl.validate((valid, fields) => {
    if (valid) {
      const result = calculatePrediction(formData.value)
      predictResult.value = result
      dialogVisible.value = true
    } else {
      console.log('验证失败:', fields)
    }
  })
}

const handleDialogClose = () => {
  dialogVisible.value = false
}
</script>

<template>
  <el-form
    ref="formRef"
    :model="formData"
    :rules="rules"
    label-width="120px"
    status-icon
  >
    <h3>第一位</h3>
    <el-form-item label="姓名" prop="name1">
      <el-input v-model="formData.name1" placeholder="请输入姓名" />
    </el-form-item>
    <el-form-item label="生日" prop="birthday1">
      <el-date-picker
        v-model="formData.birthday1"
        type="date"
        placeholder="选择日期"
        format="YYYY-MM-DD"
        value-format="YYYY-MM-DD"
      />
    </el-form-item>
    <el-form-item label="性别" prop="gender1">
      <el-radio-group v-model="formData.gender1">
        <el-radio label="male">男</el-radio>
        <el-radio label="female">女</el-radio>
      </el-radio-group>
    </el-form-item>

    <h3>第二位</h3>
    <el-form-item label="姓名" prop="name2">
      <el-input v-model="formData.name2" placeholder="请输入姓名" />
    </el-form-item>
    <el-form-item label="生日" prop="birthday2">
      <el-date-picker
        v-model="formData.birthday2"
        type="date"
        placeholder="选择日期"
        format="YYYY-MM-DD"
        value-format="YYYY-MM-DD"
      />
    </el-form-item>
    <el-form-item label="性别" prop="gender2">
      <el-radio-group v-model="formData.gender2">
        <el-radio label="male">男</el-radio>
        <el-radio label="female">女</el-radio>
      </el-radio-group>
    </el-form-item>

    <el-form-item>
      <el-button type="primary" @click="submitForm(formRef)">开始预测</el-button>
    </el-form-item>
  </el-form>

  <PredictResult
    v-model:visible="dialogVisible"
    :result="predictResult"
    @close="handleDialogClose"
  />
</template>

<style scoped>
h3 {
  margin: 20px 0;
  color: #409EFF;
}

:deep(.el-form-item) {
  margin-bottom: 22px;
}

:deep(.el-date-picker) {
  width: 100%;
}
</style> 