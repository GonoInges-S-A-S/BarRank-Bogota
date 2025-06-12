<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-4">Gestión de Imágenes</h1>

    <!-- Formulario para crear o editar imagen -->
    <form @submit.prevent="guardarImagen" class="bg-white p-4 rounded shadow mb-6">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div>
          <label class="block text-sm font-medium">Bar</label>
          <select v-model="nuevaImagen.bar_id"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400">
            <option disabled value="">Selecciona un bar</option>
            <option v-for="bar in bares" :key="bar.id" :value="bar.id">{{ bar.nombre }}</option>
          </select>
        </div>

        <div class="md:col-span-2">
          <label class="block text-sm font-medium">URL de la Imagen</label>
          <input v-model="nuevaImagen.url_imagen" type="text"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>

        <div class="md:col-span-2">
          <label class="block text-sm font-medium">Descripción</label>
          <textarea v-model="nuevaImagen.descripcion"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400"></textarea>
        </div>

        <div class="md:col-span-2">
          <label class="block text-sm font-medium">Fecha de Subida</label>
          <input v-model="nuevaImagen.fecha_subida" type="date"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>
      </div>

      <button type="submit"
        class="mt-4 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded">
        {{ nuevaImagen.id ? 'Actualizar Imagen' : 'Crear Imagen' }}
      </button>
    </form>

    <!-- Tabla de imágenes -->
    <table class="min-w-full bg-white rounded shadow text-sm">
      <thead>
        <tr class="bg-gray-100">
          <th class="text-left p-3 border-b">Bar</th>
          <th class="text-left p-3 border-b">URL</th>
          <th class="text-left p-3 border-b">Descripción</th>
          <th class="text-left p-3 border-b">Fecha</th>
          <th class="text-left p-3 border-b">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="imagen in imagenes" :key="imagen.id" class="hover:bg-gray-50">
          <td class="p-3 border-b">{{ imagen.bar?.nombre }}</td>
          <td class="p-3 border-b">{{ imagen.url_imagen }}</td>
          <td class="p-3 border-b">{{ imagen.descripcion }}</td>
          <td class="p-3 border-b">{{ imagen.fecha_subida }}</td>
          <td class="p-3 border-b space-x-2">
            <button @click="editarImagen(imagen)" class="text-blue-600 hover:underline">Editar</button>
            <button @click="eliminarImagen(imagen.id)" class="text-red-600 hover:underline">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const imagenes = ref([])
const bares = ref([])

const nuevaImagen = ref({
  bar_id: '',
  url_imagen: '',
  descripcion: '',
  fecha_subida: ''
})

const cargarImagenes = async () => {
  const res = await fetch('http://localhost:3000/api/imagenes')
  imagenes.value = await res.json()
}

const cargarBares = async () => {
  const res = await fetch('http://localhost:3000/api/bares')
  bares.value = await res.json()
}

const guardarImagen = async () => {
  const metodo = nuevaImagen.value.id ? 'PUT' : 'POST'
  const url = nuevaImagen.value.id
    ? `http://localhost:3000/api/imagenes/${nuevaImagen.value.id}`
    : 'http://localhost:3000/api/imagenes'

  await fetch(url, {
    method: metodo,
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(nuevaImagen.value)
  })

  await cargarImagenes()
  nuevaImagen.value = { bar_id: '', url_imagen: '', descripcion: '', fecha_subida: '' }
}

const editarImagen = (imagen) => {
  nuevaImagen.value = { ...imagen }
}

const eliminarImagen = async (id) => {
  if (!confirm('¿Estás seguro de eliminar esta imagen?')) return
  await fetch(`http://localhost:3000/api/imagenes/${id}`, { method: 'DELETE' })
  await cargarImagenes()
}

onMounted(() => {
  cargarImagenes()
  cargarBares()
})
</script>
