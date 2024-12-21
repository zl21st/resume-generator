<template>
  <h1>简历大师</h1>
    <div >
      <div class="container">
      <form @submit.prevent="generateResume" class="form-container">
        <label for="name">姓名:</label>
        <input v-model="formData.name" type="text" id="name" required />

        <label for="email">邮箱:</label>
        <input v-model="formData.email" type="email" id="email" required />

        <label for="phone">电话:</label>
        <input v-model="formData.phone" type="tel" id="phone" required />

        <label for="education">教育背景:</label>
        <textarea  rows="5" v-model="formData.education" id="education"></textarea>

        <label for="experience">工作经验:</label>
        <textarea rows="5" v-model="formData.experience" id="experience"></textarea>

        <button type="submit">生成简历</button>
      </form>
    </div>

    <div class="container">
      <div v-if="resumeVisible" class="resume-preview">
        <h2>简历预览</h2>
        <p><strong>姓名:</strong> {{ formData.name }}</p>
        <p><strong>邮箱:</strong> {{ formData.email }}</p>
        <p><strong>电话:</strong> {{ formData.phone }}</p>
        <p><strong>教育背景:</strong> {{ formData.education }}</p>
        <p><strong>工作经验:</strong> {{ formData.experience }}</p>

        <button @click="downloadPDF">下载PDF</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import jsPDF from 'jspdf'

const formData = ref({
  name: '',
  email: '',
  phone: '',
  education: '',
  experience: '',
})

const resumeVisible = ref(false)

const generateResume = () => {
  resumeVisible.value = true
}

const downloadPDF = () => {
  const doc = new jsPDF()

  doc.addFont('/fonts/NotoSansSC-Regular.ttf', 'NotoSansSC', 'normal')
  doc.setFont('NotoSansSC')

  doc.text(`姓名: ${formData.value.name}`, 10, 10)
  doc.text(`邮箱: ${formData.value.email}`, 10, 20)
  doc.text(`电话: ${formData.value.phone}`, 10, 30)
  doc.text(`教育背景: ${formData.value.education}`, 10, 40)
  doc.text(`工作经验: ${formData.value.experience}`, 10, 50)

  doc.save('resume.pdf')
}
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
}

.form-container {
  width: 50%;
  max-width: 600px;
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  text-align: left;
}

label {
  font-weight: bold;
}

input,
textarea {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.resume-preview {
  /* display: flex; */
  justify-content: center;
  text-align: left;
  width: 50%;
  max-width: 600px;
}
</style>
