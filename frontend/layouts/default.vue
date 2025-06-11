<template>
  <div class="min-h-screen flex flex-col bg-gray-100 text-gray-800">
    <!-- Navbar -->
    <header class="bg-indigo-900 text-white shadow-md">
      <div class="container mx-auto px-4 py-4 flex justify-between items-center">
        <h1 class="text-2xl font-bold">BarRank Bogotá</h1>
        <nav class="space-x-4">
          <NuxtLink to="/" class="hover:underline text-gray-200">Inicio</NuxtLink>
          <NuxtLink to="/bares" class="hover:underline text-gray-200">Bares</NuxtLink>
          <NuxtLink to="/usuarios" class="hover:underline text-gray-200">Usuarios</NuxtLink>
          <NuxtLink to="/resenas" class="hover:underline text-gray-200">Reseñas</NuxtLink>
        </nav>
      </div>
    </header>

      <div class="p-10">
    <button class="bg-blue-500 text-white px-4 py-2 rounded">
      Botón de prueba Tailwind
    </button>
    </div>

    <!-- Main content -->
    <main class="flex-grow container mx-auto px-4 py-8">
      <NuxtPage />
    </main>

    <!-- Footer -->
    <footer class="bg-indigo-900 text-white text-center py-4">
      © 2025 BarRank Bogotá. Todos los derechos reservados.
    </footer>
  </div>
</template>
