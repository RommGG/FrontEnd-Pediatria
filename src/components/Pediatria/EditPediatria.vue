<template>
  <div class="max-w-3xl mx-auto bg-white p-6 rounded-lg shadow-lg">
    <h2 class="text-2xl font-bold mb-6">Editar Información del Bebé</h2>

    <div v-if="message" class="text-green-600 mb-4">
      {{ message }}
    </div>

    <form @submit.prevent="submitForm">
      <!-- Sección 1: Información del Bebé -->
      <div v-if="currentStep === 1">
        <label for="sexo" class="block text-sm font-medium text-gray-700">Sexo :</label>
        <input type="text" id="sexo" v-model="bebe.sexo" required
          class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">

        <label for="fechaNacimiento" class="block text-sm font-medium text-gray-700 mt-4">Fecha de Nacimiento:</label>
        <input type="date" id="fechaNacimiento" v-model="bebe.fecha_nacimiento" required
          class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">

        <label for="horaNacimiento" class="block text-sm font-medium text-gray-700 mt-4">Hora de Nacimiento:</label>
        <input type="time" id="horaNacimiento" v-model="bebe.hora_nacimiento" step="1" required
          class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">

        <label for="lugarNacimiento" class="block text-sm font-medium text-gray-700 mt-4">Lugar de Nacimiento:</label>
        <select id="lugarNacimiento" v-model="bebe.lugar_nacimiento" required
          class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
          <option value="">Seleccionar</option>
          <option value="Xicotepec">Xicotepec</option>
          <option value="Huachinango">Huachinango</option>
          <option value="Necaxa">Necaxa</option>
          <option value="Villa Avila Camacho">Villa Avila Camacho</option>
        </select>
      </div>

      <!-- Sección 2: Información de los Padres -->
      <div v-if="currentStep === 2">
        <label for="nPadre" class="block text-sm font-medium text-gray-700">Nombre del Padre:</label>
        <input type="text" id="nPadre" v-model="bebe.nombre_padre" required
          class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">

        <label for="nMadre" class="block text-sm font-medium text-gray-700 mt-4">Nombre de la Madre:</label>
        <input type="text" id="nMadre" v-model="bebe.nombre_madre" required
          class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">

        <label for="telefonoContacto" class="block text-sm font-medium text-gray-700 mt-4">Teléfono de Contacto:</label>
        <input type="text" id="telefonoContacto" v-model="bebe.telefono_contacto" required
          class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">

        <label for="emailC" class="block text-sm font-medium text-gray-700 mt-4">Email de Contacto:</label>
        <input type="email" id="emailC" v-model="bebe.email_contacto" required
          class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">

        <label for="observaciones" class="block text-sm font-medium text-gray-700 mt-4">Observaciones:</label>
        <textarea id="observaciones" v-model="bebe.observaciones"
          class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"></textarea>
      </div>

      <!-- Sección 3: Frecuencias y Medidas -->
      <div v-if="currentStep === 3">
        <div class="grid grid-cols-2 gap-4">
          <div>
            <label for="frecuenciaCardiaca" class="block text-sm font-medium text-gray-700">Frecuencia Cardíaca:</label>
            <input type="number" id="frecuenciaCardiaca" v-model="bebe.frecuencia_cardiaca"
              class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
          </div>

          <div>
            <label for="temperatura" class="block text-sm font-medium text-gray-700">Temperatura:</label>
            <input type="number" id="temperatura" step="0.01" v-model="bebe.temperatura"
              class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
          </div>

          <div>
            <label for="presionArterialSistolica" class="block text-sm font-medium text-gray-700">Presión Arterial
              Sistólica:</label>
            <input type="number" id="presionArterialSistolica" v-model="bebe.presion_arterial_sistolica"
              class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
          </div>

          <div>
            <label for="presionArterialDiastolica" class="block text-sm font-medium text-gray-700">Presión Arterial
              Diastólica:</label>
            <input type="number" id="presionArterialDiastolica" v-model="bebe.presion_arterial_diastolica"
              class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
          </div>

          <label for="observaciones" class="block text-sm font-medium text-gray-700">Observaciones:</label><br>
      <textarea id="observaciones" v-model="bebe.observaciones"></textarea><br><br>
        </div>
      </div>

      <!-- Navegación entre pasos -->
      <div class="flex justify-between mt-6">
        <button type="button" @click="prevStep" v-if="currentStep > 1"
          class="bg-gray-600 text-white py-2 px-4 rounded-md">Anterior</button>
        <button type="button" @click="nextStep" v-if="currentStep < 3"
          class="bg-blue-600 text-white py-2 px-4 rounded-md ml-auto">Siguiente</button>
        <button type="submit" v-if="currentStep === 3" class="bg-green-600 text-white py-2 px-4 rounded-md">Guardar
          Cambios</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bebe: {
        id: '',
        sexo: '',
        fecha_nacimiento: '',
        hora_nacimiento: '',
        lugar_nacimiento: '',
        peso: '',
        longitud: '',
        nombre_padre: '',
        nombre_madre: '',
        telefono_contacto: '',
        email_contacto: '',
        observaciones: '',
        tipo_nacimiento: '',
        frecuencia_cardiaca: '',
        temperatura: '',
        presion_arterial_sistolica: '',
        presion_arterial_diastolica: ''
      },
      message: '',
      currentStep: 1 // Añadir un estado para rastrear el paso actual
    };
  },
  mounted() {
    this.obtenerDatosBebe();
  },
  methods: {
    obtenerDatosBebe() {
      const token =
        'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJOb21icmVfVXN1YXJpbyI6InlhaXIiLCJDb3JyZW9fRWxlY3Ryb25pY28iOiJzdHJpbmciLCJDb250cmFzZW5hIjoiMTIzNCIsIk51bWVyb19UZWxlZm9uaWNvX01vdmlsIjoic3RyaW5nIn0.aEXy_fgDdUHif1wzhfpxddKVg4fWAyGR3fd1p-SWDOc';
      const id = this.$route.params.id;
      fetch(`https://privilegecare-deploy.onrender.com/pediatria/nacimiento/${id}/`, {
        method: 'GET',
        headers: {
          Authorization: `Bearer ${token}`,
          'Content-Type': 'application/json'
        }
      })
        .then((response) => response.json())
        .then((data) => {
          this.bebe = data;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    submitForm() {
      const token =
        'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJOb21icmVfVXN1YXJpbyI6InlhaXIiLCJDb3JyZW9fRWxlY3Ryb25pY28iOiJzdHJpbmciLCJDb250cmFzZW5hIjoiMTIzNCIsIk51bWVyb19UZWxlZm9pY28iOiJzdHJpbmciLCJUb2tlbiI6InN0cmluZyJ9.S3LJ-t3wrX74bWViBDqA7noIqHMM9YkB3KpeiuC4wQo';
      fetch(`https://privilegecare-deploy.onrender.com/pediatria/nacimiento/${this.bebe.id}/`, {
        method: 'PUT',
        headers: {
          Authorization: `Bearer ${token}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.bebe)
      })
        .then((response) => response.json())
        .then((data) => {
          this.message = 'Información del bebé actualizada con éxito';
        })
        .catch((error) => {
          console.error(error);
        });
    },
    nextStep() {
      this.currentStep++;
    },
    prevStep() {
      this.currentStep--;
    }
  }
};
</script>
