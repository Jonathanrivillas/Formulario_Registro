<template>
  <section class="space-y-6">
    <h2 class="text-lg font-semibold text-gray-800">Información personal</h2>

    <!-- País -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">País</label>
      <div class="relative">
        <select
          v-model="form.country"
          @blur="touch('country')"
          class="w-full border rounded-lg p-2 pr-10 shadow-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        >
          <option value="">Seleccione un país</option>
          <option v-for="c in countries" :key="c" :value="c">{{ c }}</option>
        </select>
        <ValidationIcon :valid="countryValid" v-if="touched.country" />
      </div>
    </div>

    <!-- Género -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">Género</label>
      <div class="relative">
        <select
          v-model="form.gender"
          @blur="touch('gender')"
          class="w-full border rounded-lg p-2 pr-10 shadow-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        >
          <option value="">Seleccione</option>
          <option>Masculino</option>
          <option>Femenino</option>
          <option>Otro</option>
        </select>
        <ValidationIcon :valid="genderValid" v-if="touched.gender" />
      </div>
    </div>

    <!-- Primer nombre -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">Nombres</label>
      <div class="relative">
        <input
          v-model="form.firstName"
          @blur="touch('firstName')"
          class="w-full border rounded-lg p-2 pr-10 shadow-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        />
        <ValidationIcon :valid="firstNameValid" v-if="touched.firstName" />
      </div>
    </div>

    <!-- Apellidos -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">Apellidos</label>
      <div class="relative">
        <input
          v-model="form.secondName"
          @blur="touch('secondName')"
          class="w-full border rounded-lg p-2 pr-10 shadow-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        />
        <ValidationIcon :valid="secondNameValid" v-if="touched.secondName" />
      </div>
    </div>

    <!-- Fecha de nacimiento -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">Fecha de nacimiento</label>
      <div class="relative">
        <input
          type="date"
          v-model="form.dob"
          @blur="touch('dob')"
          class="w-full border rounded-lg p-2 pr-10 shadow-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        />
        <ValidationIcon :valid="dobValid" v-if="touched.dob" />
      </div>
      <p v-if="touched.dob && !dobValid" class="text-xs text-red-600 mt-1">Debe ser mayor de 18 años</p>
    </div>

    <!-- Tipo de documento -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">Tipo de documento</label>
      <div class="relative">
        <select
          v-model="form.docType"
          @blur="touch('docType')"
          class="w-full border rounded-lg p-2 pr-10 shadow-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        >
          <option value="">Seleccione</option>
          <option>Cédula de ciudadanía</option>
          <option>Pasaporte</option>
          <option>Cédula de extranjería</option>
        </select>
        <ValidationIcon :valid="docTypeValid" v-if="touched.docType" />
      </div>
    </div>

    <!-- Número de documento -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">Número documento</label>
      <div class="relative">
        <input
          v-model="form.docNumber"
          @blur="touch('docNumber')"
          class="w-full border rounded-lg p-2 pr-10 shadow-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
        />
        <ValidationIcon :valid="docNumberValid" v-if="touched.docNumber" />
      </div>
      <p v-if="touched.docNumber && !docNumberValid" class="text-xs text-red-600 mt-1">
        Solo números, mínimo 5 dígitos
      </p>
    </div>

        <!-- Foto documento Frente -->
    <div>
    <label class="block text-sm font-medium mb-1">Foto documento – Frente (JPG)</label>
    <label class="file-upload">
        <span>Seleccionar archivo</span>
        <input 
        type="file" 
        accept="image/jpeg" 
        @change="onFileChange($event, 'docFront')" 
        hidden 
        />
    </label>
    <p class="text-sm mt-1 text-gray-700">
        <span v-if="form.docFront">{{ form.docFront.name }}</span>
        <span v-else>No se ha seleccionado archivo</span>
    </p>
    <p class="text-sm mt-1">
        <span v-if="touched.docFront">{{ docFrontValid ? '✅ Archivo válido' : '❌ Solo JPG permitido' }}</span>
    </p>
    </div>

    <!-- Foto documento Reverso -->
    <div>
    <label class="block text-sm font-medium mb-1">Foto documento – Reverso (JPG)</label>
    <label class="file-upload">
        <span>Seleccionar archivo</span>
        <input 
        type="file" 
        accept="image/jpeg" 
        @change="onFileChange($event, 'docBack')" 
        hidden 
        />
    </label>
    <p class="text-sm mt-1 text-gray-700">
        <span v-if="form.docBack">{{ form.docBack.name }}</span>
        <span v-else>No se ha seleccionado archivo</span>
    </p>
    <p class="text-sm mt-1">
        <span v-if="touched.docBack">{{ docBackValid ? '✅ Archivo válido' : '❌ Solo JPG permitido' }}</span>
    </p>
    </div>

    </section>
    </template>

<script>
import ValidationIcon from "./ValidationIcon.vue"

export default {
  props: ["form", "touched", "countries"],
  emits: ["touch", "fileChange"],
  components: { ValidationIcon },
  computed: {
    countryValid() { return this.form.country.trim() !== '' },
    genderValid() { return this.form.gender.trim() !== '' },
    firstNameValid() { return this.form.firstName.trim().length > 0 },
    secondNameValid() { return this.form.secondName.trim().length > 0 },
    dobValid() {
      if (!this.form.dob) return false
      const birth = new Date(this.form.dob)
      const today = new Date()
      let age = today.getFullYear() - birth.getFullYear()
      const m = today.getMonth() - birth.getMonth()
      if (m < 0 || (m === 0 && today.getDate() < birth.getDate())) age--
      return age >= 18
    },
    docTypeValid() { return this.form.docType.trim() !== '' },
    docNumberValid() { return /^\d{5,}$/.test(this.form.docNumber) },
    docFrontValid() { return this.form.docFront && this.form.docFront.type === 'image/jpeg' },
    docBackValid() { return this.form.docBack && this.form.docBack.type === 'image/jpeg' },
  },
  methods: {
    touch(field) { this.$emit("touch", field) },
    onFileChange(e, which) { this.$emit("fileChange", e, which) }
  }
}
</script>
