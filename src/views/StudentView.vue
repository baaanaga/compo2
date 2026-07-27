<script setup lang="ts">
import StudentCard from '@/components/StudentCard.vue'
import type { Student } from '@/types'
import { ref, onMounted } from 'vue'
import StudentService from '@/services/StudentService'

const events = ref<Student[] | null>(null)

onMounted(() => {
  StudentService.getEvents()
    .then((response) => {
      events.value = response.data
      // console.log(response.data)
    })
    .catch((error) => {
      console.error('There was an error!', error)
    })
})
</script>

<template>
  <h1>Student</h1>
  <div class="">
    <div class="event-row" v-for="event in events" :key="event.id">
      <StudentCard :student="event" />
    </div>
  </div>
</template>
