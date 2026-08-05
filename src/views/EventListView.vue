<script setup lang="ts">
import EventCard from '@/components/EventCard.vue'
import CaOr from '@/components/CaOr.vue'
import type { Event } from '@/types'
import { ref, onMounted, computed, watchEffect } from 'vue'
import EventService from '@/services/EventService'
import { useRoute, useRouter } from 'vue-router'

const router = useRouter()
const route = useRoute()

const events = ref<Event [] | null> (null)
const totalEvents = ref(0)

const page = computed(() => props.page)
const size = computed(() => props.size)

const hasNextPage = computed(()=> {
  const totalPages = Math.ceil(totalEvents.value / size.value)
  return page.value < totalPages
})
const props = defineProps({
  page: {
    type: Number,
    required: true
  },
  size: {
    type: Number,
    required: true
  }
})

const sizeInput = ref(props.size)

function updateSize() {
  if (!sizeInput.value || sizeInput.value < 1) return
  router.push({
    name: 'event-list-view',
    query: { ...route.query, size: sizeInput.value, page: 1 }
  })
}

onMounted(()=> {
  watchEffect(() => {
    sizeInput.value = props.size
    events.value = null
    EventService.getEvents(size.value, page.value)
      .then((response) => {
        events.value = response.data
        totalEvents.value = response.headers['x-total-count']
        // console.log(response.data)
      })
      .catch((error) => {
        console.error('There was an error!', error)
    })
  })
})
</script>

<template>

  <div class="page-size">
    <label for="size-input">Events per page:</label>
    <input
      id="size-input"
      type="number"
      list="size-options"
      v-model.number="sizeInput"
      @change="updateSize"
      min="1"
    />
    <datalist id="size-options">
      <option value="2" />
      <option value="5" />
      <option value="10" />
      <option value="20" />
    </datalist>
  </div>

<h1>Events For Good</h1>

<div class="events">  
  <div class="event-row" v-for="event in events" :key="event.id">
    <EventCard :event="event" />
    <CaOr :event="event" />
  </div>
  <div class="pagination">
    <RouterLink
      id="page-prev"
      :to="{ name: 'event-list-view', query: { page: page - 1, size: size } }"
      rel="prev"
      v-if="page != 1"
      >&#60; Prev Page
    </RouterLink>

    <RouterLink
    id="page-next"
      :to="{ name: 'event-list-view', query: { page: page + 1, size: size } }"
      rel="next"
      v-if="hasNextPage"
      >Next Page &#62;
    </RouterLink>
  </div>
</div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pagination {
  display: flex;
  width: 290px;
}

.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}

#page-prev {
  text-align: left;
}

#page-next {
  text-align: right;
}

.page-size {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
  color: #2c3e50;
}

.page-size label {
  font-size: 1rem;
}

.page-size input {
  width: 60px;
  font-size: 1rem;
  color: #2c3e50;
  border: 1px solid #2c3e50;
  border-radius: 4px;
  padding: 2px 6px;
}
</style>
