<script setup>
import { ref } from 'vue'

const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
const schedule = ref({
  Monday: [],
  Tuesday: [],
  Wednesday: [],
  Thursday: [],
  Friday: []
})

const newLesson = ref({
  day: '',
  course: '',
  time: ''
})

function addLesson() {
  if (newLesson.value.day && newLesson.value.course && newLesson.value.time) {
    schedule.value[newLesson.value.day].push({
      course: newLesson.value.course,
      time: newLesson.value.time
    })
    newLesson.value.day = ''
    newLesson.value.course = ''
    newLesson.value.time = ''
  }
}
</script>

<template>
  <div class="kontainer">
    <h1>Buat Jadwal Mata Kuliah Anda</h1>
    <div class="input-data">
      <label class="hari">
        Hari:
        <select v-model="newLesson.day">
          <option disabled value="">Pilih Hari</option>
          <option v-for="day in days" :key="day">{{ day }}</option>
        </select>
      </label>
      <label class="matkul">
        Mata Kuliah:
        <input v-model="newLesson.course" placeholder="Nama Mata Kuliah" />
      </label>
      <label class="jam">
        Jam:
        <input v-model="newLesson.time" placeholder="Jam" />
      </label>
      <button @click="addLesson">Tambah</button>
    </div>
    <div class="schedule-grid" :class="{ 'scrollable-grid': isScrollable }">
      <div v-for="day in days" :key="day" class="schedule-card">
        <h2>{{ day }}</h2>
        <div v-if="schedule[day].length > 0" class="lesson-container">
          <div v-for="(lesson, index) in schedule[day]" :key="index" class="lesson">
            <p :class="{ evening: lesson.time.includes('PM') }">
              {{ lesson.course }} - {{ lesson.time }}
            </p>
          </div>
        </div>
        <p v-else>Belum Ada Jadwal</p>
      </div>
    </div>
  </div>
</template>

<style scoped>

.input-data{
    align-items: center;
    text-align: center;
    justify-content: center;
    background-color: transparent;
    border: 1px solid rgba(255,255,255,2);
    backdrop-filter: blur(24px);
    color: black;
    border-radius: 12px;
    padding: 24px 28px;
    margin-bottom: 16px;
}

.input-data button{
  width: 25%;
  height: 32px;
    background: rgb(71, 71, 79);
    border: none;
    outline: none;
    border-radius: 40px;
    margin-top: 16px;
    box-shadow: 0 0 12px rgba(0,0,0,.1);
}

.hari, .matkul, .jam {
  display: block;
  margin-bottom: 0.5rem;
  text-align: start;
}

.hari select, .matkul input, .jam input {
  width: 100%;
  padding: 0.5rem;
  font-size: 1rem;
  border-radius: 1rem;
  border: 1px solid #ccc;
}

.kontainer{
  background: #f5f5f5;
  align-items: center;
  text-align: center;
  justify-content: center;
  max-width: 50rem;
  margin: 3.75rem auto;
  height: 78rem;
  padding: 1.25rem;
  border-radius: 16px;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.3);
}

.schedule-grid {
  display: flex;
  overflow-x: auto;
  gap: 20px;
  padding-bottom: 20px;
}

.schedule-card {
  width: 250px;
  border: 1px solid #ccc;
  border-radius: 0.5rem;
  padding: 1rem;
}

.lesson-container {
  max-height: 300px; 
  overflow-y: auto;
}

.lesson {
  margin-bottom: 0.5rem;
}

.evening {
  color: rgb(70, 66, 66);
  font-weight: bold;
}

.scrollable-grid {
  justify-content: flex-start;
}
</style>
