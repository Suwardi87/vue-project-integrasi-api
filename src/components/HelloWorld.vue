<template>
  <div>
    <h2>Daftar Tempat Wisata</h2>
    <ul v-if="wisata.length">
      <li v-for="item in wisata" :key="item.id">
        <strong>{{ item.nama }}</strong> - {{ item.lokasi }}<br>
        <em>{{ item.deskripsi }}</em>
      </li>
    </ul>
    <p v-else>Loading data...</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const wisata = ref([])

onMounted(async () => {
  try {
    const response = await axios.get('http://localhost:3000/wisata')
    wisata.value = response.data
  } catch (error) {
    console.error('Gagal fetch data:', error)
  }
})
</script>
