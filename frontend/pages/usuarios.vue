<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-4">Gestión de Usuarios</h1>

    <!-- Formulario para crear o editar usuario -->
    <form @submit.prevent="guardarUsuario" class="bg-white p-4 rounded shadow mb-6">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div class="md:col-span-2">
          <label class="block text-sm font-medium">Nombre</label>
          <input v-model="nuevoUsuario.nombre" type="text"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>

        <div class="md:col-span-2">
          <label class="block text-sm font-medium">Correo</label>
          <input v-model="nuevoUsuario.correo" type="email"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>

        <div class="md:col-span-2">
          <label class="block text-sm font-medium">Edad</label>
          <input v-model="nuevoUsuario.edad" type="number"
            class="w-full border border-gray-300 rounded p-2 focus:outline-none focus:ring-2 focus:ring-blue-400" />
        </div>
      </div>

      <button type="submit"
        class="mt-4 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded">
        {{ nuevoUsuario.id ? 'Actualizar Usuario' : 'Crear Usuario' }}
      </button>
    </form>

    <!-- Tabla de usuarios -->
    <table class="min-w-full bg-white rounded shadow text-sm">
      <thead>
        <tr class="bg-gray-100">
          <th class="text-left p-3 border-b">Nombre</th>
          <th class="text-left p-3 border-b">Correo</th>
          <th class="text-left p-3 border-b">Edad</th>
          <th class="text-left p-3 border-b">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="usuario in usuarios" :key="usuario.id" class="hover:bg-gray-50">
          <td class="p-3 border-b">{{ usuario.nombre }}</td>
          <td class="p-3 border-b">{{ usuario.correo }}</td>
          <td class="p-3 border-b">{{ usuario.edad }}</td>
          <td class="p-3 border-b space-x-2">
            <button @click="editarUsuario(usuario)" class="text-blue-600 hover:underline">Editar</button>
            <button @click="eliminarUsuario(usuario.id)" class="text-red-600 hover:underline">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const usuarios = ref([])

const nuevoUsuario = ref({
  nombre: '',
  correo: '',
  edad: ''
})

const cargarUsuarios = async () => {
  const res = await fetch('http://localhost:3000/api/usuarios')
  usuarios.value = await res.json()
}

const guardarUsuario = async () => {
  const metodo = nuevoUsuario.value.id ? 'PUT' : 'POST'
  const url = nuevoUsuario.value.id
    ? `http://localhost:3000/api/usuarios/${nuevoUsuario.value.id}`
    : 'http://localhost:3000/api/usuarios'

  await fetch(url, {
    method: metodo,
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(nuevoUsuario.value)
  })

  await cargarUsuarios()
  nuevoUsuario.value = { nombre: '', correo: '', edad: '' }
}

const editarUsuario = (usuario) => {
  nuevoUsuario.value = { ...usuario }
}

const eliminarUsuario = async (id) => {
  if (!confirm('¿Estás seguro de eliminar este usuario?')) return
  await fetch(`http://localhost:3000/api/usuarios/${id}`, { method: 'DELETE' })
  await cargarUsuarios()
}

onMounted(() => {
  cargarUsuarios()
})
</script>
