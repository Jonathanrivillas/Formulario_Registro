<template>
  <section class="space-y-6">
    <h2 class="text-lg font-semibold text-gray-800">Dirección</h2>

    <div class="relative">
      <label class="block text-sm font-medium text-gray-700 mb-1">Dirección residencia</label>
      <input
        v-model="form.address"
        @blur="onBlur('address')"
        autocomplete="street-address"
        class="w-full border rounded-lg p-3 pr-10 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-400"
      />
      <ValidationIcon :valid="addressValid" v-if="touched.address" />
      <p v-if="touched.address && !addressValid" class="text-xs text-red-600 mt-1">Campo obligatorio</p>
    </div>

    <div class="relative">
      <label class="block text-sm font-medium text-gray-700 mb-1">Código postal</label>
      <input
        v-model="form.postal"
        @blur="onBlur('postal')"
        inputmode="numeric"
        class="w-full border rounded-lg p-3 pr-10 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-400"
      />
      <ValidationIcon :valid="postalValid" v-if="touched.postal" />
      <p v-if="touched.postal && !postalValid" class="text-xs text-red-600 mt-1">Campo obligatorio</p>
    </div>
  </section>
</template>

<script>
import ValidationIcon from "./ValidationIcon.vue"

export default {
  props: ["form", "touched"],
  emits: ["touch"],
  components: { ValidationIcon },
  computed: {
    addressValid() { return (this.form.address || '').trim().length > 0 },
    postalValid() { return (this.form.postal || '').trim().length > 0 }
  },
  methods: {
    onBlur(field) { this.$emit('touch', field) }
  }
}
</script>
