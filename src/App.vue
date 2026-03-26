<template>
  <main class="app">
    <section class="tracker-card">
      <div class="tracker-header">
        <p class="tracker-label">Mini proyecto Vue</p>
        <h1>Habit Tracker</h1>
        <p class="tracker-subtitle">
          Organiza tus hábitos diarios de forma simple y visual.
        </p>
      </div>

      <div class="tracker-form">
        <input
          v-model="nuevoHabito"
          type="text"
          placeholder="Escribe un nuevo hábito..."
          class="tracker-input"
        />

        <button @click="agregarHabito" class="tracker-add-button">
          Agregar
        </button>
      </div>

      <div class="tracker-info">
        <p class="tracker-resumen">
          Has completado <strong>{{ habitosCompletados }}</strong> de
          <strong>{{ habitos.length }}</strong> hábitos
        </p>
      </div>

      <p v-if="habitos.length === 0" class="tracker-empty">
        Aún no agregas hábitos.
      </p>

      <ul class="tracker-list">
        <li
          v-for="(habito, index) in habitos"
          :key="index"
          class="tracker-item"
        >
          <label class="tracker-item-left">
            <input type="checkbox" v-model="habito.completado" />
            <span :class="{ completado: habito.completado }">
              {{ habito.texto }}
            </span>
          </label>

          <button
            @click="eliminarHabito(index)"
            class="tracker-delete-button"
            aria-label="Eliminar hábito"
          >
            ✕
          </button>
        </li>
      </ul>
    </section>
  </main>
</template>

<script setup>
import { ref, watch, onMounted, computed } from 'vue'

const nuevoHabito = ref('')
const habitos = ref([])

const agregarHabito = () => {
  if (nuevoHabito.value.trim() === '') return

  habitos.value.push({
    texto: nuevoHabito.value,
    completado: false
  })

  nuevoHabito.value = ''
}

const eliminarHabito = (index) => {
  habitos.value.splice(index, 1)
}

const habitosCompletados = computed(() => {
  return habitos.value.filter((h) => h.completado).length
})

watch(
  habitos,
  (nuevoValor) => {
    localStorage.setItem('habitos', JSON.stringify(nuevoValor))
  },
  { deep: true }
)

onMounted(() => {
  const datos = localStorage.getItem('habitos')
  if (datos) {
    habitos.value = JSON.parse(datos)
  }
})
</script>