<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-4">Gestión de Categorías</h1>

    <!-- Formulario para crear o editar categoría -->
    <form @submit.prevent="guardarCategoria" class="bg-white p-4 rounded shadow mb-6">
      <div class="mb-4">
        <label class="block text-sm font-medium">Nombre de la categoría</label>
        <input
          v-model="nuevaCategoria.nombre"
          type="text"
          class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400"
          placeholder="Ej: Rooftop, Rock, Electrónica"
        />
      </div>

      <button
        type="submit"
        class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded"
      >
        {{ nuevaCategoria.id ? 'Actualizar Categoría' : 'Crear Categoría' }}
      </button>
    </form>

    <!-- Tabla de categorías -->
    <table class="min-w-full bg-white rounded shadow">
      <thead>
        <tr>
          <th class="text-left p-2 border-b">ID</th>
          <th class="text-left p-2 border-b">Nombre</th>
          <th class="text-left p-2 border-b">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="categoria in categorias" :key="categoria.id" class="hover:bg-gray-100">
          <td class="p-2 border-b">{{ categoria.id }}</td>
          <td class="p-2 border-b">{{ categoria.nombre }}</td>
          <td class="p-2 border-b space-x-2">
            <button
              @click="nuevaCategoria = { ...categoria }"
              class="text-blue-600 hover:underline"
            >Editar</button>
            <button
              @click="eliminarCategoria(categoria.id)"
              class="text-red-600 hover:underline"
            >Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const categorias = ref([])
const nuevaCategoria = ref({ nombre: '' })

const cargarCategorias = async () => {
  try {
    const res = await fetch('http://localhost:3000/api/categorias')
    categorias.value = await res.json()
  } catch (error) {
    console.error('Error cargando categorías:', error)
  }
}

const guardarCategoria = async () => {
  const metodo = nuevaCategoria.value.id ? 'PUT' : 'POST'
  const url = nuevaCategoria.value.id
    ? `http://localhost:3000/api/categorias/${nuevaCategoria.value.id}`
    : 'http://localhost:3000/api/categorias'

  try {
    await fetch(url, {
      method: metodo,
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(nuevaCategoria.value)
    })
    await cargarCategorias()
    nuevaCategoria.value = { nombre: '' }
  } catch (error) {
    console.error('Error guardando categoría:', error)
  }
}

const eliminarCategoria = async (id) => {
  if (!confirm('¿Estás seguro de eliminar esta categoría?')) return
  try {
    await fetch(`http://localhost:3000/api/categorias/${id}`, {
      method: 'DELETE'
    })
    await cargarCategorias()
  } catch (error) {
    console.error('Error eliminando categoría:', error)
  }
}

onMounted(cargarCategorias)
</script>