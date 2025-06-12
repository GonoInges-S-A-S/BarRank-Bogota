<template>
  <div class="bg-white text-gray-900">
    <!-- Hero -->
    <section class="min-h-screen flex flex-col justify-center items-center text-center bg-gradient-to-br from-indigo-600 to-blue-500 text-white px-6">
      <h1 class="text-5xl font-extrabold mb-4">Bienvenido a BarRank Bogotá 🍻</h1>
      <p class="text-xl mb-8 max-w-xl">Descubre, califica y comparte los mejores bares de Bogotá según instalaciones, atención y más.</p>
      <NuxtLink to="/bares" class="bg-white text-indigo-600 font-semibold px-6 py-3 rounded-lg shadow hover:bg-gray-100 transition">
        Explora Bares
      </NuxtLink>
    </section>

    <!-- Sección Info -->
    <section class="py-16 px-8 bg-gray-100 text-center">
      <h2 class="text-3xl font-bold mb-6">¿Qué ofrecemos?</h2>
      <div class="grid md:grid-cols-3 gap-8">
        <div>
          <h3 class="text-xl font-semibold mb-2">Ranking Inteligente</h3>
          <p>Calificaciones reales de usuarios para tomar decisiones confiables.</p>
        </div>
        <div>
          <h3 class="text-xl font-semibold mb-2">Fotos y Opiniones</h3>
          <p>Imágenes y comentarios que muestran el ambiente real de cada bar.</p>
        </div>
        <div>
          <h3 class="text-xl font-semibold mb-2">Filtros Avanzados</h3>
          <p>Encuentra bares por tipo de música, ubicación, atención y más.</p>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white py-6 text-center">
      <p class="text-sm">&copy; 2025 BarRank Bogotá - GonoInges S.A.S</p>
    </footer>
  </div>
</template>
