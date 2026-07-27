<script setup lang="ts">
import type { Student } from '@/types'
defineProps<{
  student: Student
}>()
</script>

<template>
  <div class="event-class">
    <div class="event-card">
    <p>Name: {{ student.name }}</p>
    <p>Surname: {{ student.surname }}</p>
    <p>GPA: {{ student.gpa }}</p>
    </div>
  </div>
</template>

<style scoped>
.event-class {
  display: flex;
  justify-content: center;
}

.event-card {
  padding: 20px;
  width: 250px;
  cursor: pointer;
  border: 1px solid #39495c;
  margin-bottom: 18px;
}

.event-card:hover {
  transform: scale(1.01);
  box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2);
}
</style>