<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-4">Gestión de Bares</h1>

    <!-- Formulario para crear o editar un bar -->
    <form @submit.prevent="guardarBar" class="bg-white p-4 rounded shadow mb-6">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div>
          <label class="block text-sm font-medium">Nombre</label>
          <input v-model="nuevoBar.nombre" type="text"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>

        <div>
          <label class="block text-sm font-medium">Dirección</label>
          <input v-model="nuevoBar.direccion" type="text"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>

        <div>
          <label class="block text-sm font-medium">Ciudad</label>
          <input v-model="nuevoBar.ciudad" type="text"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>

        <div>
          <label class="block text-sm font-medium">Descripción</label>
          <input v-model="nuevoBar.descripcion" type="text"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>

        <div>
          <label class="block text-sm font-medium">Categoría</label>
          <select v-model="nuevoBar.categoria_id"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400">
            <option disabled value="">Selecciona una categoría</option>
            <option v-for="cat in categorias" :key="cat.id" :value="cat.id">{{ cat.nombre }}</option>
          </select>
        </div>
      </div>

      <button type="submit"
        class="mt-4 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded">
        {{ nuevoBar.id ? 'Actualizar Bar' : 'Crear Bar' }}
      </button>
    </form>

    <!-- Tabla de bares -->
    <table class="min-w-full bg-white rounded shadow text-sm">
      <thead>
        <tr class="bg-gray-100">
          <th class="text-left p-3 border-b">Nombre</th>
          <th class="text-left p-3 border-b">Ciudad</th>
          <th class="text-left p-3 border-b">Dirección</th>
          <th class="text-left p-3 border-b">Categoría</th>
          <th class="text-left p-3 border-b">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="bar in bares" :key="bar.id" class="hover:bg-gray-50">
          <td class="p-3 border-b">{{ bar.nombre }}</td>
          <td class="p-3 border-b">{{ bar.ciudad }}</td>
          <td class="p-3 border-b">{{ bar.direccion }}</td>
          <td class="p-3 border-b">{{ bar.categoria?.nombre || 'Sin categoría' }}</td>
          <td class="p-3 border-b space-x-2">
            <button @click="editarBar(bar)" class="text-blue-600 hover:underline">Editar</button>
            <button @click="eliminarBar(bar.id)" class="text-red-600 hover:underline">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const bares = ref([])
const categorias = ref([])

const nuevoBar = ref({
  nombre: '',
  direccion: '',
  ciudad: '',
  descripcion: '',
  categoria_id: ''
})

const cargarBares = async () => {
  try {
    const res = await fetch('http://localhost:3000/api/bares')
    bares.value = await res.json()
  } catch (error) {
    console.error('Error cargando bares:', error)
  }
}

const cargarCategorias = async () => {
  try {
    const res = await fetch('http://localhost:3000/api/categorias')
    categorias.value = await res.json()
  } catch (error) {
    console.error('Error cargando categorías:', error)
  }
}

const guardarBar = async () => {
  const metodo = nuevoBar.value.id ? 'PUT' : 'POST'
  const url = nuevoBar.value.id
    ? `http://localhost:3000/api/bares/${nuevoBar.value.id}`
    : 'http://localhost:3000/api/bares'

  try {
    await fetch(url, {
      method: metodo,
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(nuevoBar.value)
    })
    await cargarBares()
    nuevoBar.value = {
      nombre: '',
      direccion: '',
      ciudad: '',
      descripcion: '',
      categoria_id: ''
    }
  } catch (error) {
    console.error('Error guardando bar:', error)
  }
}

const editarBar = (bar) => {
  nuevoBar.value = { ...bar }
}

const eliminarBar = async (id) => {
  if (!confirm('¿Estás seguro de eliminar este bar?')) return
  try {
    await fetch(`http://localhost:3000/api/bares/${id}`, {
      method: 'DELETE'
    })
    await cargarBares()
  } catch (error) {
    console.error('Error eliminando bar:', error)
  }
}

onMounted(() => {
  cargarBares()
  cargarCategorias()
})
</script>