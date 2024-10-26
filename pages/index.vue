<template>
  <div>
    <h1>Position actuelle</h1>
    <p>Latitude: {{ position.latitude }}</p>
    <p>Longitude: {{ position.longitude }}</p>
    <p>Précision: {{ position.accuracy }} mètres</p>
  </div>
</template>

<script lang="ts" setup>
import { Geolocation } from "@capacitor/geolocation";

const position = ref({
  latitude: 0,
  longitude: 0,
  accuracy: 0,
});

const frequency = 10 * 1000;

const getPosition = async () => {
  try {
    const coordinates = await Geolocation.getCurrentPosition();
    position.value = {
      latitude: coordinates.coords.latitude,
      longitude: coordinates.coords.longitude,
      accuracy: coordinates.coords.accuracy,
    };
  } catch (error) {}
};

let positionInterval: ReturnType<typeof setInterval>;

onMounted(() => {
  getPosition();
  positionInterval = setInterval(getPosition, frequency);
});

onUnmounted(() => {
  clearInterval(positionInterval);
});
</script>

<style></style>
