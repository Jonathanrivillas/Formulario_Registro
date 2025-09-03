<template>
  <div class="min-h-screen flex items-center justify-center bg-gradient-to-r from-blue-900 to-gray-900 p-4">
    <div class="w-full max-w-3xl bg-white rounded-2xl shadow-2xl p-8">
      <!-- Título -->
      <h1 class="text-3xl font-bold text-gray-800 text-center mb-2">Registro</h1>
      <p class="text-gray-500 text-center mb-6">Completa los 3 pasos para crear tu cuenta</p>

      <!-- Step Indicator -->
      <StepIndicator :currentStep="currentStep" class="mb-8" />

      <!-- Paso 1 -->
      <Step1 v-if="currentStep === 1"
        :form="form" :touched="touched" :countries="countries"
        @touch="touch" @fileChange="onFileChange" />

      <!-- Paso 2 -->
      <Step2 v-if="currentStep === 2"
        :form="form" :touched="touched"
        @touch="touch" />

      <!-- Paso 3 -->
      <Step3 v-if="currentStep === 3"
        :form="form" :touched="touched"
        @touch="touch" />

      <!-- Botones -->
      <div class="flex justify-between mt-8">
        <button
          :disabled="currentStep === 1"
          @click="prevStep"
          class="px-6 py-2 rounded-lg border border-gray-400 text-gray-600 
                 hover:bg-gray-100 transition disabled:opacity-40 flex items-center gap-2"
        >
          ← Atrás
        </button>

        <div>
          <button v-if="currentStep < 3"
            @click="nextStep"
            :disabled="!isCurrentStepValid"
            class="px-6 py-2 rounded-lg bg-gradient-to-r from-blue-600 to-blue-800 
                   text-white font-semibold shadow hover:scale-105 transition 
                   disabled:opacity-40 flex items-center gap-2"
          >
            Siguiente →
          </button>

          <button v-else
            @click="submitForm"
            :disabled="!isCurrentStepValid"
            class="px-6 py-2 rounded-lg bg-gradient-to-r from-green-500 to-green-700 
                   text-white font-semibold shadow hover:scale-105 transition 
                   disabled:opacity-40 flex items-center gap-2"
          >
            Enviar ✅
          </button>
        </div>
      </div>
    </div>

    <!-- Modal -->
    <Modal v-if="showModal" :form="form" @close="showModal=false" />
  </div>
</template>

<script>
import StepIndicator from "./components/StepIndicator.vue"
import Step1 from "./components/Step1.vue"
import Step2 from "./components/Step2.vue"
import Step3 from "./components/Step3.vue"
import Modal from "./components/Modal.vue"

export default {
  components: { StepIndicator, Step1, Step2, Step3, Modal },
  data() {
    return {
      currentStep: 1,
      showModal: false,
      countries: [],
      form: {
        country: '', gender: '', firstName: '', secondName: '', dob: '',
        docType: '', docNumber: '', docFront: null, docBack: null,
        email: '', password: '', passwordConfirm: '',
        phone: '', mobile: '', address: '', postal: ''
      },
      touched: {}
    }
  },
  computed: {
    isCurrentStepValid() {
      if (this.currentStep === 1) {
        return this.form.country && this.form.gender && this.form.firstName &&
               this.form.secondName && this.form.dob && this.isAdult &&
               this.form.docType && /^\d{5,}$/.test(this.form.docNumber) &&
               this.form.docFront && this.form.docBack
      }
      if (this.currentStep === 2) {
        return this.isValidEmail(this.form.email) &&
               this.form.password.length >= 6 &&
               this.form.password === this.form.passwordConfirm &&
               /^\d+$/.test(this.form.phone) &&
               /^\d+$/.test(this.form.mobile)
      }
      if (this.currentStep === 3) {
        return this.form.address.trim().length > 0 &&
               this.form.postal.trim().length > 0
      }
      return false
    },
    isAdult() {
      if (!this.form.dob) return false
      const birth = new Date(this.form.dob)
      const today = new Date()
      let age = today.getFullYear() - birth.getFullYear()
      const m = today.getMonth() - birth.getMonth()
      if (m < 0 || (m === 0 && today.getDate() < birth.getDate())) age--
      return age >= 18
    }
  },
  methods: {
    touch(field) {
      this.touched = { ...this.touched, [field]: true }
    },
    onFileChange(e, which) {
      const file = e.target.files && e.target.files[0]
      if (which === 'docFront') this.form.docFront = file
      if (which === 'docBack') this.form.docBack = file
      this.touch(which)
    },
    nextStep() {
      if (this.isCurrentStepValid && this.currentStep < 3) this.currentStep++
    },
    prevStep() {
      if (this.currentStep > 1) this.currentStep--
    },
    submitForm() {
      if (this.isCurrentStepValid) {
        console.log("FORM DATA:", this.form)
        this.showModal = true
      }
    },
    async fetchCountries() {
      try {
        const res = await fetch("https://countriesnow.space/api/v0.1/countries/positions")
        if (!res.ok) {
          throw new Error(`HTTP error! status: ${res.status}`)
        }
        const data = await res.json()
        this.countries = data.data.map(c => c.name).sort()
      } catch (e) {
        console.error("Error al obtener países:", e)
        this.countries = ["Argentina", "Brasil", "Chile", "Colombia", "México", "Perú", "Uruguay"]
      }
    },
    isValidEmail(email) {
      const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
      return re.test(email.trim())
    }
  },
  mounted() { this.fetchCountries() }
}
</script>
