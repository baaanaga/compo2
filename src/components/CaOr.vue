<script setup lang="ts">
import type { Event } from '@/types'
defineProps<{
  event: Event
}>()
</script>

<template>
  <div class="CaOr">
    <p class="info-text">{{ event.category }}</p>
    <p class="info-text">{{ event.organizer }}</p>
  </div>
</template>

<style scoped>
.CaOr {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  text-align: right;
}

.info-text {
  font-size: 16px;
  margin: 2px 0;
}
</style>