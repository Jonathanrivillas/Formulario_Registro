<template>
  <span class="absolute right-3 top-1/2 -translate-y-1/2 pointer-events-none flex items-center">
    <svg v-if="valid"
         xmlns="http://www.w3.org/2000/svg"
         viewBox="0 0 24 24"
         fill="none"
         stroke="currentColor"
         stroke-width="2"
         style="width:20px;height:20px;"
         class="text-green-500">
      <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7" />
    </svg>

    <svg v-else
         xmlns="http://www.w3.org/2000/svg"
         viewBox="0 0 24 24"
         fill="none"
         stroke="currentColor"
         stroke-width="2"
         style="width:20px;height:20px;"
         class="text-red-500">
      <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
    </svg>
  </span>
</template>

<script>
export default {
  props: {
    valid: { type: Boolean, required: true }
  }
}
</script>
