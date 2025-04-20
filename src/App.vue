<template>
  <div class="p-4">
    <h1 class="text-xl font-bold mb-4"> Scheduling passengers</h1>

    <div v-for="trip in trips" :key="trip.id" class="mb-4 border p-3 rounded bg-gray-100">
      <h2 class="font-semibold">Trip {{ trip.id }} - Departs at {{ trip.departureTime }}:00</h2>
      <p>Seats Left: {{ trip.seatsAvailable }}</p>
      <ol class="list-disc list-inside">
        <li v-for="p in trip.passengers" :key="p.name">{{ p.name }}</li>
      </ol>
    </div>

    <button @click="runScheduling" class="mt-4 px-4 py-2 bg-blue-600 text-white rounded">Run Scheduling</button>

    <div v-if="unscheduled.length" class="mt-4 text-red-600">
      <p>Unscheduled Passengers:</p>
      <ol>
        <li v-for="name in unscheduled" :key="name">{{ name }}</li>
      </ol>
      <button @click="unscheduled = []" class="mt-2 text-sm text-blue-600">Clear Unscheduled List</button>

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const trips = ref([


{ id: 'T1', departureTime: 8, seatsAvailable: 0, passengers: [] },
  { id: 'T2', departureTime: 9, seatsAvailable: 2, passengers: [] },
  { id: 'T3', departureTime: 10, seatsAvailable: 3, passengers: [] },

])

const passengers = ref([


  { name: 'mike', preferredTime: 9 },

  { name: 'liz', preferredTime: 8 },
  { name: 'john', preferredTime: 11 },
  { name: 'omondi', preferredTime: 10 },
])

const unscheduled = ref([])

function schedulePassenger(passenger, tripList, index = 0) {
  
  if (index >= tripList.length) {
    unscheduled.value.push(passenger.name)
    return
  }

  const trip = tripList[index]
  if (trip.departureTime >= passenger.preferredTime && trip.seatsAvailable > 0) {
  
  
  
    trip.passengers.push(passenger)
    trip.seatsAvailable--
  } 
  
  
  else {
    schedulePassenger(passenger, tripList, index + 1)
  }
}

function runScheduling() {


  trips.value.forEach(trip => {
  
  
      trip.passengers = []
    trip.seatsAvailable = trip.id === 'T2' ? 2 : trip.id === 'T3' ? 3 : 0
  })
  unscheduled.value = []

  passengers.value.forEach(passenger => {
    schedulePassenger(passenger, trips.value)
  })
}
</script>

<style scoped>
body {
  font-family: 'Segoe UI', sans-serif;
}
</style>
