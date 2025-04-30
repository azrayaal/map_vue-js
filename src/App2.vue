<template>
  <div>
    <l-map
      style="height: 500px; width: 100%"
      :zoom="18"
      :center="center"
      @click="updatePosition"
    >
      <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png" />

      <!-- Marker bisa diseret -->
      <l-marker :lat-lng="center" :draggable="true" @moveend="onMarkerDrag" />

      <!-- Lingkaran radius -->
      <l-circle :lat-lng="center" :radius="radius" color="blue" />
    </l-map>

    <!-- Radius slider -->
    <div style="margin-top: 1rem">
      <label>
        Radius: {{ radius }} meter
        <input type="range" min="100" max="2000" step="100" v-model="radius" />
      </label>
    </div>

    <!-- Zona waktu berdasarkan koordinat -->
    <div style="margin-top: 1rem">
      <p>Koordinat: {{ center[0].toFixed(5) }}, {{ center[1].toFixed(5) }}</p>
      <p>Zona Waktu: {{ getTimezone(center[1]) }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { LMap, LTileLayer, LMarker, LCircle } from "@vue-leaflet/vue-leaflet";

const center = ref([-6.1754, 106.8272]);
const radius = ref(100);

function getTimezone(lng) {
  if (lng >= 95 && lng < 110) return "WIB (GMT+7)";
  else if (lng >= 110 && lng < 135) return "WITA (GMT+8)";
  else if (lng >= 135) return "WIT (GMT+9)";
  else return "Luar Indonesia";
}

// Saat user klik peta, ubah posisi marker dan radius
function updatePosition(event) {
  const { lat, lng } = event.latlng;
  center.value = [lat, lng];
}

// Saat user menyeret marker, ubah posisi marker dan radius
function onMarkerDrag(event) {
  const { lat, lng } = event.target.getLatLng();
  center.value = [lat, lng];
}
</script>
