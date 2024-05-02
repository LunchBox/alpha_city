<script setup>
import { GoogleMap, Marker } from 'vue3-google-map'
const center = { lat: 40.689247, lng: -74.044502 }
</script>

<template>
  <div>
    <GoogleMap
      api-key="---"
      style="width: 100%; height: 500px"
      :center="center"
      :zoom="15"
      >
      <Marker :options="{ position: center }" />
    </GoogleMap>
  </div>
</template>

<style scoped>
</style>
