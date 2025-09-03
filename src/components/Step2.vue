<template>
  <section class="space-y-6">
    <h2 class="text-lg font-semibold text-gray-800">Contacto</h2>

    <!-- Email -->
    <div class="relative">
      <label class="block text-sm font-medium text-gray-700 mb-1">Correo electrónico</label>
      <input
        v-model="form.email"
        @blur="onBlur('email')"
        type="email"
        autocomplete="email"
        class="w-full border rounded-lg p-3 pr-10 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-400"
      />
      <ValidationIcon :valid="emailValid" v-if="touched.email" />
      <p v-if="touched.email && !emailValid" class="text-xs text-red-600 mt-1">Formato de correo inválido</p>
    </div>

    <!-- Contraseña -->
    <div class="relative">
      <label class="block text-sm font-medium text-gray-700 mb-1">Contraseña</label>
      <input
        v-model="form.password"
        @blur="onBlur('password')"
        type="password"
        autocomplete="new-password"
        class="w-full border rounded-lg p-3 pr-10 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-400"
      />
      <ValidationIcon :valid="passwordValid" v-if="touched.password" />
      <p v-if="touched.password && !passwordValid" class="text-xs text-red-600 mt-1">Mínimo 6 caracteres</p>
    </div>

    <!-- Confirmación -->
    <div class="relative">
      <label class="block text-sm font-medium text-gray-700 mb-1">Confirmar contraseña</label>
      <input
        v-model="form.passwordConfirm"
        @blur="onBlur('passwordConfirm')"
        type="password"
        autocomplete="new-password"
        class="w-full border rounded-lg p-3 pr-10 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-400"
      />
      <ValidationIcon :valid="passwordMatch" v-if="touched.passwordConfirm" />
      <p v-if="touched.passwordConfirm && !passwordMatch" class="text-xs text-red-600 mt-1">Las contraseñas no coinciden</p>
    </div>

    <!-- Teléfonos -->
    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
      <div class="relative">
        <label class="block text-sm font-medium text-gray-700 mb-1">Número teléfono</label>
        <input
          v-model="form.phone"
          @blur="onBlur('phone')"
          inputmode="numeric"
          pattern="\d*"
          autocomplete="tel"
          class="w-full border rounded-lg p-3 pr-10 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-400"
        />
        <ValidationIcon :valid="phoneValid" v-if="touched.phone" />
        <p v-if="touched.phone && !phoneValid" class="text-xs text-red-600 mt-1">Solo dígitos</p>
      </div>

      <div class="relative">
        <label class="block text-sm font-medium text-gray-700 mb-1">Número celular</label>
        <input
          v-model="form.mobile"
          @blur="onBlur('mobile')"
          inputmode="numeric"
          pattern="\d*"
          autocomplete="tel"
          class="w-full border rounded-lg p-3 pr-10 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-400"
        />
        <ValidationIcon :valid="mobileValid" v-if="touched.mobile" />
        <p v-if="touched.mobile && !mobileValid" class="text-xs text-red-600 mt-1">Solo dígitos</p>
      </div>
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
    emailValid() {
      const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
      return re.test((this.form.email || '').trim())
    },
    passwordValid() { return (this.form.password || '').length >= 6 },
    passwordMatch() { return this.form.password && this.form.passwordConfirm && this.form.password === this.form.passwordConfirm },
    phoneValid() { return this.form.phone === '' || /^\d+$/.test(this.form.phone) },
    mobileValid() { return this.form.mobile === '' || /^\d+$/.test(this.form.mobile) }
  },
  methods: {
    onBlur(field) { this.$emit('touch', field) }
  }
}
</script>
