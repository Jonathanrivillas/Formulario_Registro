<template>
  <div class="mb-8">
    <div class="flex items-center gap-4">
      <div v-for="(step, idx) in steps" :key="idx" class="flex-1">
        <div class="flex items-center">
          <div
            :class="[
              'w-11 h-11 rounded-full flex items-center justify-center text-sm font-semibold shadow-md transition-all duration-300',
              currentStep === idx+1 ? 'bg-blue-600 text-white scale-105' : (currentStep > idx+1 ? 'bg-green-600 text-white' : 'bg-gray-200 text-gray-700')
            ]"
          >
            <span v-if="currentStep > idx+1">✔</span>
            <span v-else>{{ idx+1 }}</span>
          </div>
          <div v-if="idx < steps.length - 1" :class="['flex-1 h-1 mx-3 transition-colors', currentStep > idx+1 ? 'bg-green-600' : 'bg-gray-200']"></div>
        </div>
        <div class="mt-2 text-xs text-center text-gray-500">{{ step }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: { currentStep: { type: Number, required: true } },
  data() {
    return {
      steps: ['Información', 'Contacto', 'Dirección']
    }
  }
}
</script>
