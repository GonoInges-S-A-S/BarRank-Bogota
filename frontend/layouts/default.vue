<template>
  <div>
    <header class="bg-white shadow p-4 flex justify-between items-center">
      <h1 class="text-xl font-bold">BarRank Bogotá</h1>
      <nav>
        <NuxtLink to="/" class="text-indigo-600 hover:underline mr-4">Inicio</NuxtLink>
        <NuxtLink to="/categorias" class="text-indigo-600 hover:underline">Categorias   </NuxtLink>
        <NuxtLink to="/bares" class="text-indigo-600 hover:underline">Bares   </NuxtLink>
        <NuxtLink to="/resenas" class="text-indigo-600 hover:underline">Reseñas   </NuxtLink>
        <NuxtLink to="/usuarios" class="text-indigo-600 hover:underline">Usuarios   </NuxtLink>
        <NuxtLink to="/imagenes" class="text-indigo-600 hover:underline">Imagenes   </NuxtLink>
      </nav>
    </header>

    <main>
      <slot />
    </main>
  </div>
</template>
