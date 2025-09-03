<template>
  <div class="fixed inset-0 bg-black/50 flex items-center justify-center z-50">
    <div class="bg-white rounded-xl p-8 max-w-md w-full text-center shadow-lg animate-fade-in">
      <!-- Icon -->
      <svg class="w-16 h-16 text-green-500 mx-auto mb-4" fill="none" stroke="currentColor" stroke-width="2"
           viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round"
              d="M9 12l2 2l4-4m6 2a9 9 0 11-18 0a9 9 0 0118 0z"/>
      </svg>

      <h3 class="text-2xl font-semibold mb-2">¡Enviado con éxito!</h3>
      <p class="text-gray-600 mb-6">El formulario fue enviado correctamente.</p>

      <button
        @click="$emit('close')"
        class="px-6 py-2 bg-blue-600 text-white rounded-lg shadow hover:bg-blue-700 transition"
      >
        Cerrar
      </button>
    </div>
  </div>
</template>

<style>
@keyframes fade-in {
  from { opacity: 0; transform: scale(0.9); }
  to { opacity: 1; transform: scale(1); }
}
.animate-fade-in {
  animation: fade-in 0.3s ease-out;
}
</style>
