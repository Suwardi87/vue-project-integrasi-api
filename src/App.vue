<template>
  <div>
    <h2>{{ isEdit ? 'Edit Wisata' : 'Tambah Wisata' }}</h2>
    <form @submit.prevent="isEdit ? updateWisata() : tambahWisata()">
      <input v-model="form.nama" placeholder="Nama Wisata" required />
      <input v-model="form.lokasi" placeholder="Lokasi" required />
      <textarea v-model="form.deskripsi" placeholder="Deskripsi" required></textarea>
      <button type="submit">{{ isEdit ? 'Update' : 'Tambah' }}</button>
      <button type="button" @click="resetForm" v-if="isEdit">Batal</button>
    </form>

    <hr />

    <h2>Daftar Wisata</h2>
    <ul>
      <li v-for="item in wisata" :key="item.id">
        <h3>{{ item.nama }}</h3>
        <p><strong>Lokasi:</strong> {{ item.lokasi }}</p>
        <p>{{ item.deskripsi }}</p>
        <button @click="editItem(item)">Edit</button>
        <button @click="hapusWisata(item.id)">Hapus</button>
        <hr />
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const wisata = ref([])
const form = ref({ id: null, nama: '', lokasi: '', deskripsi: '' })
const isEdit = ref(false) // untuk menandai apakah sedang edit

// Ambil data saat component dimuat
onMounted(async () => {
  await getData()
})

// Ambil data dari server
const getData = async () => {
  const res = await axios.get('http://localhost:3000/wisata')
  wisata.value = res.data
}

// Tambah data baru
const tambahWisata = async () => {
  await axios.post('http://localhost:3000/wisata', {
    nama: form.value.nama,
    lokasi: form.value.lokasi,
    deskripsi: form.value.deskripsi,
  })
  resetForm()
  await getData()
}

// Hapus data
const hapusWisata = async (id) => {
  await axios.delete(`http://localhost:3000/wisata/${id}`)
  await getData()
}

// Edit data: isi form & aktifkan mode edit
const editItem = (item) => {
  form.value = { ...item }
  isEdit.value = true
}

// Update data (PUT)
const updateWisata = async () => {
  await axios.put(`http://localhost:3000/wisata/${form.value.id}`, {
    nama: form.value.nama,
    lokasi: form.value.lokasi,
    deskripsi: form.value.deskripsi,
  })
  resetForm()
  await getData()
}

// Reset form ke kondisi awal
const resetForm = () => {
  form.value = { id: null, nama: '', lokasi: '', deskripsi: '' }
  isEdit.value = false
}
</script>
