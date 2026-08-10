<script setup lang="ts">
import { onMounted, toRefs } from 'vue'
import { type Event } from '@/types'
import { useMessageStore } from '@/stores/message';
import { storeToRefs } from 'pinia';
const props = defineProps<{
  event: Event
}>()
const { event } = toRefs(props)
const store = useMessageStore()
const { message } = storeToRefs(store)

onMounted(() => {
  if (message.value) {
    setTimeout(() => {
      store.resetMessage()
    }, 3000)
  }
})
</script>

<template>
  <div v-if="message" class="flash-message">
    <h4>{{ message }}</h4>
  </div>
  <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
  <p>{{ event.description }}</p>
</template>

<style scoped>
.flash-message {
  animation: yellowFade 3s ease-in-out;
}

@keyframes yellowFade {
  from {
    background-color: yellow;
  }
  to {
    background-color: transparent;
  }
}
</style>
