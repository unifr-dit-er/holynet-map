<script setup lang="ts">
defineProps<{
  msg: string
}>()

import { ref, onMounted } from 'vue'
import 'leaflet/dist/leaflet.css'
import * as L from 'leaflet'
import 'leaflet.markercluster/dist/MarkerCluster.css'
import 'leaflet.markercluster/dist/MarkerCluster.Default.css'
import 'leaflet.markercluster'
import { addressPoints } from './markerDemo'

const initialMap = ref(null)

onMounted(() => {
  initialMap.value = L.map('map', {
    zoomControl: true,
    zoom: 1,
    zoomAnimation: false,
    fadeAnimation: true,
    markerZoomAnimation: true,
  }).setView([23.8041, 90.4152], 6)

  L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
  }).addTo(initialMap.value)
  L.marker([24.3746, 88.6004]).addTo(initialMap.value)
  L.marker([22.3752, 91.8349]).addTo(initialMap.value)

  const markers = L.markerClusterGroup()
  addressPoints.forEach((element) => {
    const each_marker = new L.marker([element.latitude, element.longitude])
    markers.addLayer(each_marker)
  })

  initialMap.value.addLayer(markers)
})
</script>

<template>
  <div class="leaflet-container">
    <h1>{{ msg }}</h1>
    <div id="map">The map here</div>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
}

#map {
  height: 90vh;
  width: 60vw;
}
@media (min-width: 1024px) {
}
</style>
