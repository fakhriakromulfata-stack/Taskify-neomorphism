<script setup>
    import { ref, computed } from 'vue'

    const tasks = ref([])
    const inputTask = ref('')
    const selectedPriority = ref('normal')
    const currentFilter = ref('all')

    const addTask = (title) => {
      if (!title.trim()) return

      const newTask = {
        id: new Date().getTime(),
        title: title,
        status: false,
        priority: selectedPriority.value
      }

      tasks.value.push(newTask)
      inputTask.value = ''
    }

    const deleteTask = (id) => {
      const task = tasks.value.find(item => item.id === id)

      if (!task){
        alert("Task tidak ditemukan!")
        return
      }

      const confirmation = confirm("Apakah anda yakin")

      if (confirmation) {
        tasks.value = tasks.value.filter(item => item.id !== id)
      }
    }

    // Computed properties untuk kompleksitas tambahan (statistik & filter)
    const totalTasks = computed(() => tasks.value.length)
    const completedTasks = computed(() => tasks.value.filter(t => t.status).length)
    const pendingTasks = computed(() => tasks.value.filter(t => !t.status).length)

    const filteredTasks = computed(() => {
      if (currentFilter.value === 'completed') return tasks.value.filter(t => t.status)
      if (currentFilter.value === 'pending') return tasks.value.filter(t => !t.status)
      return tasks.value
    })
</script>

<template>
  <div class="min-h-screen bg-[#e3d5c7] flex items-center justify-center p-4">
    <section class="mx-auto w-full max-w-xl px-4">
        <!-- Card Utama Neumorphism -->
        <div class="rounded-3xl p-8 bg-[#e3d5c7] shadow-[12px_12px_24px_#c2b5a6,-12px_-12px_24px_#fff5e8] text-[#5c5348]">
          <h1 class="mb-6 text-center text-3xl font-extrabold tracking-wide text-[#4a4137]">Taskify veu</h1>
          
          <!-- Statistik Ringkas -->
          <div class="grid grid-cols-3 gap-4 mb-6">
            <div class="p-3 rounded-2xl bg-[#e3d5c7] text-center shadow-[inset_3px_3px_6px_#c2b5a6,inset_-3px_-3px_6px_#fff5e8]">
              <span class="block text-xs text-[#8a7d6e] font-semibold">Total</span>
              <span class="text-lg font-bold text-[#4a4137]">{{ totalTasks }}</span>
            </div>
            <div class="p-3 rounded-2xl bg-[#e3d5c7] text-center shadow-[inset_3px_3px_6px_#c2b5a6,inset_-3px_-3px_6px_#fff5e8]">
              <span class="block text-xs text-[#8a7d6e] font-semibold">Pending</span>
              <span class="text-lg font-bold text-amber-600">{{ pendingTasks }}</span>
            </div>
            <div class="p-3 rounded-2xl bg-[#e3d5c7] text-center shadow-[inset_3px_3px_6px_#c2b5a6,inset_-3px_-3px_6px_#fff5e8]">
              <span class="block text-xs text-[#8a7d6e] font-semibold">Selesai</span>
              <span class="text-lg font-bold text-emerald-600">{{ completedTasks }}</span>
            </div>
          </div>

          <!-- Form Input & Prioritas -->
          <form @submit.prevent="addTask(inputTask)" action="" class="space-y-4">
            <div class="flex gap-3">
              <input
                v-model="inputTask"
                type="text"
                placeholder="Masukkan tugas baru..."
                aria-label="Tugas baru"
                class="min-w-0 flex-1 rounded-2xl bg-[#e3d5c7] px-4 py-3 outline-none text-[#4a4137] placeholder-[#8a7d6e] shadow-[inset_4px_4px_8px_#c2b5a6,inset_-4px_-4px_8px_#fff5e8]"
              />
              <button type="submit" class="rounded-2xl bg-[#e3d5c7] px-6 py-3 font-semibold text-red-500 shadow-[5px_5px_10px_#c2b5a6,-5px_-5px_10px_#fff5e8] active:shadow-[inset_3px_3px_6px_#c2b5a6,inset_-3px_-3px_6px_#fff5e8] transition">
                Tambah
              </button>
            </div>

            <!-- Pilihan Prioritas -->
            <div class="flex items-center justify-between px-2 text-sm text-[#7a6d5e]">
              <span class="font-medium">Prioritas:</span>
              <div class="flex gap-4">
                <label class="flex items-center gap-1 cursor-pointer">
                  <input type="radio" value="low" v-model="selectedPriority" class="accent-red-500" /> Rendah
                </label>
                <label class="flex items-center gap-1 cursor-pointer">
                  <input type="radio" value="normal" v-model="selectedPriority" class="accent-red-500" /> Sedang
                </label>
                <label class="flex items-center gap-1 cursor-pointer">
                  <input type="radio" value="high" v-model="selectedPriority" class="accent-red-500" /> Tinggi
                </label>
              </div>
            </div>
          </form>

          <!-- Filter Tab -->
          <div class="flex gap-2 mt-6 p-1 rounded-xl bg-[#e3d5c7] shadow-[inset_2px_2px_5px_#c2b5a6,inset_-2px_-2px_5px_#fff5e8]">
            <button 
              @click="currentFilter = 'all'" 
              :class="currentFilter === 'all' ? 'shadow-[3px_3px_6px_#c2b5a6,-3px_-3px_6px_#fff5e8] text-[#4a4137] font-bold' : 'text-[#8a7d6e]'"
              class="flex-1 py-1.5 text-xs rounded-lg transition">Semua</button>
            <button 
              @click="currentFilter = 'pending'" 
              :class="currentFilter === 'pending' ? 'shadow-[3px_3px_6px_#c2b5a6,-3px_-3px_6px_#fff5e8] text-[#4a4137] font-bold' : 'text-[#8a7d6e]'"
              class="flex-1 py-1.5 text-xs rounded-lg transition">Belum</button>
            <button 
              @click="currentFilter = 'completed'" 
              :class="currentFilter === 'completed' ? 'shadow-[3px_3px_6px_#c2b5a6,-3px_-3px_6px_#fff5e8] text-[#4a4137] font-bold' : 'text-[#8a7d6e]'"
              class="flex-1 py-1.5 text-xs rounded-lg transition">Selesai</button>
          </div>

          <!-- Daftar Task -->
          <ul class="mt-6 space-y-4" v-auto-animate>
            <li v-for="task in filteredTasks" :key="task.id" class="flex items-center justify-between rounded-2xl bg-[#e3d5c7] px-4 py-3 shadow-[5px_5px_10px_#c2b5a6,-5px_-5px_10px_#fff5e8]">
              <div class="flex items-center gap-3">
                  <input 
                  v-model="task.status"
                  type="checkbox"
                  class="h-4 w-4 accent-red-500 rounded cursor-pointer" />
                  
                  <!-- Indikator Warna Prioritas -->
                  <span 
                    class="w-2.5 h-2.5 rounded-full" 
                    :class="{
                      'bg-blue-400': task.priority === 'low',
                      'bg-amber-500': task.priority === 'normal',
                      'bg-red-500': task.priority === 'high'
                    }"
                    :title="'Prioritas: ' + task.priority">
                  </span>

                  <span class="text-[#4a4137]" :class="task.status ? 'line-through text-[#9a8d7e]' : ''">
                    {{ task.title }}
                  </span>
              </div>
              <button @click="deleteTask(task.id)" class="text-red-500 hover:text-red-700 font-medium text-sm transition">Hapus</button>
            </li>
            <li v-if="filteredTasks.length === 0" class="text-center py-4 text-sm text-[#8a7d6e]">
              Tidak ada tugas ditemukan.
            </li>
          </ul>

        </div>
      </section>
  </div>
</template>