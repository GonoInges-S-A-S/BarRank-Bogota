<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-4">Gestión de Reseñas</h1>

    <!-- Formulario para crear o editar reseña -->
    <form @submit.prevent="guardarResena" class="bg-white p-4 rounded shadow mb-6">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div>
          <label class="block text-sm font-medium">Usuario</label>
          <select v-model="nuevaResena.usuario_id"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400">
            <option disabled value="">Selecciona un usuario</option>
            <option v-for="usuario in usuarios" :key="usuario.id" :value="usuario.id">{{ usuario.nombre_usuario }}</option>
          </select>
        </div>

        <div>
          <label class="block text-sm font-medium">Bar</label>
          <select v-model="nuevaResena.bar_id"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400">
            <option disabled value="">Selecciona un bar</option>
            <option v-for="bar in bares" :key="bar.id" :value="bar.id">{{ bar.nombre }}</option>
          </select>
        </div>

        <div>
          <label class="block text-sm font-medium">Puntuación Comodidad</label>
          <input v-model.number="nuevaResena.puntuacion_comodidad" type="number" min="1" max="5"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>

        <div>
          <label class="block text-sm font-medium">Puntuación Ambiente</label>
          <input v-model.number="nuevaResena.puntuacion_ambiente" type="number" min="1" max="5"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>

        <div>
          <label class="block text-sm font-medium">Puntuación Servicio</label>
          <input v-model.number="nuevaResena.puntuacion_servicio" type="number" min="1" max="5"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>

        <div class="md:col-span-2">
          <label class="block text-sm font-medium">Comentario</label>
          <textarea v-model="nuevaResena.comentario"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400"></textarea>
        </div>
      </div>

      <button type="submit"
        class="mt-4 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded">
        {{ nuevaResena.id ? 'Actualizar Reseña' : 'Crear Reseña' }}
      </button>
    </form>

    <!-- Tabla de reseñas -->
    <table class="min-w-full bg-white rounded shadow text-sm">
      <thead>
        <tr class="bg-gray-100">
          <th class="text-left p-3 border-b">Usuario</th>
          <th class="text-left p-3 border-b">Bar</th>
          <th class="text-left p-3 border-b">Comodidad</th>
          <th class="text-left p-3 border-b">Ambiente</th>
          <th class="text-left p-3 border-b">Servicio</th>
          <th class="text-left p-3 border-b">Comentario</th>
          <th class="text-left p-3 border-b">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="resena in resenas" :key="resena.id" class="hover:bg-gray-50">
          <td class="p-3 border-b">{{ resena.usuario?.nombre_usuario }}</td>
          <td class="p-3 border-b">{{ resena.bar?.nombre }}</td>
          <td class="p-3 border-b">{{ resena.puntuacion_comodidad }}</td>
          <td class="p-3 border-b">{{ resena.puntuacion_ambiente }}</td>
          <td class="p-3 border-b">{{ resena.puntuacion_servicio }}</td>
          <td class="p-3 border-b">{{ resena.comentario }}</td>
          <td class="p-3 border-b space-x-2">
            <button @click="editarResena(resena)" class="text-blue-600 hover:underline">Editar</button>
            <button @click="eliminarResena(resena.id)" class="text-red-600 hover:underline">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const resenas = ref([])
const usuarios = ref([])
const bares = ref([])

const nuevaResena = ref({
  usuario_id: '',
  bar_id: '',
  puntuacion_comodidad: 1,
  puntuacion_ambiente: 1,
  puntuacion_servicio: 1,
  comentario: ''
})

const cargarResenas = async () => {
  const res = await fetch('http://localhost:3000/api/resenas')
  resenas.value = await res.json()
}

const cargarUsuarios = async () => {
  const res = await fetch('http://localhost:3000/api/usuarios')
  usuarios.value = await res.json()
}

const cargarBares = async () => {
  const res = await fetch('http://localhost:3000/api/bares')
  bares.value = await res.json()
}

const guardarResena = async () => {
  const metodo = nuevaResena.value.id ? 'PUT' : 'POST'
  const url = nuevaResena.value.id
    ? `http://localhost:3000/api/resenas/${nuevaResena.value.id}`
    : 'http://localhost:3000/api/resenas'

  await fetch(url, {
    method: metodo,
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(nuevaResena.value)
  })

  await cargarResenas()
  nuevaResena.value = {
    usuario_id: '',
    bar_id: '',
    puntuacion_comodidad: 1,
    puntuacion_ambiente: 1,
    puntuacion_servicio: 1,
    comentario: ''
  }
}

const editarResena = (resena) => {
  nuevaResena.value = { ...resena }
}

const eliminarResena = async (id) => {
  if (!confirm('¿Estás seguro de eliminar esta reseña?')) return
  await fetch(`http://localhost:3000/api/resenas/${id}`, {
    method: 'DELETE'
  })
  await cargarResenas()
}

onMounted(() => {
  cargarResenas()
  cargarUsuarios()
  cargarBares()
})
</script>
