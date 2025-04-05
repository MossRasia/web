
<template>
  <div class="card p-4 mb-4 shadow-lg border-0 profile-card">
    <h2 class="mb-4 text-primary fw-bold">ข้อมูลนิสิต</h2>

    <div v-if="loading" class="text-center my-5">
      <div class="spinner-border text-primary" role="status"></div>
      <p class="mt-2">กำลังโหลดข้อมูล...</p>
    </div>

    <div v-else-if="student && student.name">
      <div class="d-flex justify-content-center gap-3 mb-4 flex-wrap">
        <img :src="student.image1" class="img-fluid rounded stylish-img" alt="Image 1" @error="onImageError" />
        <img :src="student.image2" class="img-fluid rounded stylish-img" alt="Image 2" @error="onImageError" />
      </div>

      <form @submit.prevent="updateStudent">
        <div class="mb-3">
          <label class="form-label fw-bold">ชื่อ-นามสกุล</label>
          <input v-model="student.name" class="form-control" />
        </div>
        <div class="mb-3">
          <label class="form-label fw-bold">รหัสนิสิต</label>
          <input v-model="student.studentId" class="form-control" />
        </div>
        <div class="mb-3">
          <label class="form-label fw-bold">สาขา</label>
          <input v-model="student.department" class="form-control" />
        </div>
        <div class="mb-3">
          <label class="form-label fw-bold">โรงเรียน</label>
          <input v-model="student.school" class="form-control" />
        </div>
        <button type="submit" class="btn btn-success px-4 mt-2">บันทึก</button>
      </form>
    </div>

    <div v-else class="text-danger text-center my-4">
      information not found
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const student = ref(null)
const loading = ref(true)
const fallbackImage = '/img/default.jpg'

const onImageError = (event) => {
  event.target.src = fallbackImage
}

onMounted(async () => {
  try {
    const res = await axios.get('http://localhost:3000/student')
    student.value = res.data
  } catch (err) {
    console.error('Error loading student:', err)
  } finally {
    loading.value = false
  }
})

const updateStudent = async () => {
  await axios.put('http://localhost:3000/student', student.value)
  alert('Data saved successfully!')
}
</script>

<style scoped>
.profile-card {
  background: linear-gradient(135deg, #1e1e2f, #2b2b3c);
  border-radius: 1rem;
}

.stylish-img {
  width: 240px;
  height: 240px;
  object-fit: cover;
  border-radius: 20px;
  box-shadow: 0 0 15px #00f0ff;
  transition: transform 0.6s ease;
}

.stylish-img:hover {
  transform: scale(1.1);
}
</style>
