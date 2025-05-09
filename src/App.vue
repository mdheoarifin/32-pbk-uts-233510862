<template>
  <div class="todo-container">
    <h1>Daftar Kegiatan</h1>

    <!-- Input untuk menambah kegiatan baru -->
    <div class="input-group">
      <input 
        v-model="newTask" 
        placeholder="Masukkan kegiatan baru" 
        @keyup.enter="addTask" 
      />
      <button @click="addTask">Tambah</button>
    </div>

    <!-- Checkbox untuk filter hanya tugas belum selesai -->
    <div class="filter-group">
      <label>
        <input type="checkbox" v-model="showOnlyPending" />
        Tampilkan hanya kegiatan belum selesai
      </label>
    </div>

    <!-- Daftar tugas hanya akan tampil setelah ditambahkan -->
    <ul v-if="filteredTasks.length > 0">
      <li v-for="task in filteredTasks" :key="task.id">
        <!-- Checkbox untuk menandai selesai -->
        <input type="checkbox" v-model="task.done" />
        <!-- Teks tugas, diberikan kelas .done jika task.done = true -->
        <span :class="{ done: task.done }">{{ task.text }}</span>
        <!-- Tombol hapus tugas -->
        <button @click="deleteTask(task.id)">Hapus</button>
      </li>
    </ul>

    <!-- Pesan jika tidak ada tugas -->
    <p v-else>Belum ada kegiatan, silakan tambahkan kegiatan.</p>
  </div>
</template>

<script setup>
// Import Vue 3 reactivity
import { ref, reactive, computed } from 'vue'

// Variabel reaktif untuk teks tugas baru
const newTask = ref('')

// Daftar tugas (array reaktif) dengan contoh awal
const tasks = reactive([])

// Opsi filter untuk menampilkan hanya tugas yang belum selesai
const showOnlyPending = ref(false)

// Fungsi untuk menambah tugas baru
function addTask() {
  const text = newTask.value.trim()
  if (text) {
    tasks.push({
      id: Date.now(), // Menggunakan timestamp sebagai ID unik
      text: text,
      done: false
    })
    newTask.value = '' // Kosongkan input setelah menambah tugas
  }
}

// Fungsi untuk menghapus tugas berdasarkan ID
function deleteTask(id) {
  const index = tasks.findIndex(task => task.id === id)
  if (index !== -1) {
    tasks.splice(index, 1) // Hapus tugas dari array
  }
}

// Properti terhitung untuk memfilter tugas berdasarkan pilihan
const filteredTasks = computed(() => {
  if (showOnlyPending.value) {
    return tasks.filter(task => !task.done)
  }
  return tasks
})
</script>

<style scoped>
/* Gaya untuk container utama */
.todo-container {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  font-family: 'Arial', sans-serif;
  color: white;
  text-align: center;
  transition: all 0.3s ease;
}

.todo-container:hover {
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.2);
}

/* Gaya untuk judul */
h1 {
  color: #fff;
  font-size: 36px;
  margin-bottom: 20px;
  font-weight: 600;
}

/* Gaya untuk grup input */
.input-group {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.input-group input {
  flex: 1;
  padding: 14px;
  font-size: 16px;
  border: 2px solid #ddd;
  border-radius: 8px;
  outline: none;
  transition: border-color 0.3s ease;
}

.input-group input:focus {
  border-color: #2575fc;
}

.input-group button {
  padding: 12px 24px;
  background-color: #4CAF50;
  color: white;
  font-size: 16px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.input-group button:hover {
  background-color: #45a049;
}

/* Gaya untuk grup filter */
.filter-group {
  margin-bottom: 20px;
}

.filter-group label {
  font-size: 16px;
  color: #fff;
}

.filter-group input {
  margin-right: 10px;
  accent-color: #4CAF50;
}

/* Gaya untuk daftar tugas */
ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #fff;
  padding: 15px;
  margin-bottom: 12px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.2s ease;
}

li:hover {
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

li input[type="checkbox"] {
  margin-right: 15px;
  cursor: pointer;
  transform: scale(1.2);
}

li span {
  flex: 1;
  font-size: 18px;
  color: #333;
  transition: color 0.3s ease;
}

.done {
  text-decoration: line-through;
  color: #888;
}

li button {
  background: #f44336;
  color: white;
  padding: 8px 16px;
  border-radius: 8px;
  border: none;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s ease;
}

li button:hover {
  background-color: #e53935;
}

/* Pesan jika tidak ada tugas */
p {
  color: #ddd;
  font-size: 18px;
}

/* Responsif untuk tampilan mobile */
@media (max-width: 600px) {
  .todo-container {
    padding: 15px;
  }

  h1 {
    font-size: 28px;
  }

  .input-group input {
    font-size: 14px;
  }

  .input-group button {
    padding: 10px 20px;
  }

  li {
    padding: 12px;
  }
}
</style>
