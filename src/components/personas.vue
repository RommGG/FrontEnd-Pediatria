<template>
  <div class="py-4 lg:py-16">
    <div class="flex flex-col lg:flex-row bg-white rounded-lg shadow-lg overflow-hidden mx-auto max-w-sm lg:max-w-7xl">
      <!-- Columna de imagen -->
      <div class="w-full lg:w-5/12 bg-gray-400 dark:bg-gray-800 bg-cover rounded-t-lg lg:rounded-l-lg hidden lg:block"
        style="background-image: url('https://i.pinimg.com/originals/ec/0e/69/ec0e690e32f7c84e714886136a2f6469.jpg')"></div>
      <!-- Columna de formulario -->
      <div class="w-full lg:w-7/12 bg-white dark:bg-gray-700 p-6 lg:p-8 rounded-lg lg:rounded-l-none">
        <h3 class="py-4 text-xl lg:text-2xl text-center text-gray-800 dark:text-white">Registro Persona</h3>
        <form class="px-4 lg:px-8 pt-6 pb-8 bg-white dark:bg-gray-800 rounded space-y-6">
          <!-- Nombre y apellidos -->
          <div class="flex flex-col lg:flex-row lg:space-x-6">
            <div class="flex-1">
              <label class="block mb-2 text-sm font-bold text-gray-700 dark:text-white" for="titulo_cortesia">
                Título de Cortesía
              </label>
              <select v-model="titulo_cortesia" id="titulo_cortesia"
              class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline">
                <option disabled selected>Título</option>
                <option value="Señor">Sr.</option>
                <option value="Señora">Sra.</option>
                <option value="Doctor">Dr.</option>
                <option value="Doctora">Dra.</option>
                <option value="Ingeniero">Ing.</option>
                <option value="Licenciad@">Lic.</option>
                <option value="Enfermera">Enf.</option>
                <option value="Qhimico">Quím.</option>
                <option value="Otro">Otro</option>
              </select>
            </div>
            <div class="flex-1">
              <label class="block mb-2 text-sm font-bold text-gray-700 dark:text-white" for="nombre">
                Nombre
              </label>
              <input v-model="nombre"
                class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
                id="nombre"
                type="text"
                placeholder="Nombre"
              />
            </div>
          </div>
          <div class="flex flex-col lg:flex-row lg:space-x-6">
            <div class="flex-1">
              <label class="block mb-2 text-sm font-bold text-gray-700 dark:text-white" for="primer_apellido">
                Primer Apellido
              </label>
              <input v-model="primer_apellido"
                class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
                id="primer_apellido"
                type="text"
                placeholder="Primer Apellido"
              />
            </div>
            <div class="flex-1">
              <label class="block mb-2 text-sm font-bold text-gray-700 dark:text-white" for="segundo_apellido">
                Segundo Apellido
              </label>
              <input v-model="segundo_apellido"
                class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
                id="segundo_apellido"
                type="text"
                placeholder="Segundo Apellido"
              />
            </div>
          </div>
          <!-- CURP, Género, Tipo de Sangre -->
          <div class="flex flex-col lg:flex-row lg:space-x-6">
            <div class="flex-1">
              <label class="block mb-2 text-sm font-bold text-gray-700 dark:text-white" for="curp">
                CURP
              </label>
              <input v-model="curp"
                class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
                id="curp"
                type="text"
                placeholder="CURP"
                maxlength="18" 
              />
            </div>
            <div class="flex-1">
              <label class="block mb-2 text-sm font-bold text-gray-700 dark:text-white" for="tipo_sangre">
                Tipo de Sangre
              </label>
              <select v-model="tipo_sangre" id="tipo_sangre"
                class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline">
                <option disabled selected>Tipo de Sangre</option>
                <option value="A+">A+</option>
                <option value="A-">A-</option>
                <option value="B+">B+</option>
                <option value="B-">B-</option>
                <option value="AB+">AB+</option>
                <option value="AB-">AB-</option>
                <option value="O+">O+</option>
                <option value="O-">O-</option>
              </select>
            </div>
            <div class="flex-1">
              <label class="block mb-2 text-sm font-bold text-gray-700 dark:text-white" for="genero">
                Género
              </label>
              <select v-model="genero" id="genero"
                class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline">
                <option disabled selected>Género</option>
                <option value="Masculino">Masculino</option>
                <option value="Femenino">Femenino</option>
                <option value="Otro">N/B</option>
              </select>
            </div>
            <div class="flex-1">
              <label for="fecha_nacimiento"
                class="block mb-2 text-sm font-bold text-gray-700 dark:text-white">Fecha de Nacimiento</label>
              <input v-model="fecha_nacimiento" id="fecha_nacimiento" name="fecha_nacimiento" type="date" required
                class="appearance-none block w-full px-3 py-2 border border-gray-300 rounded-md placeholder-gray-400 focus:outline-none focus:shadow-outline-blue focus:border-blue-300 transition duration-150 ease-in-out sm:text-sm sm:leading-5">
            </div>
          </div>
          <!-- Fotografía -->
          <div class="mb-4">
            <label class="block mb-2 text-sm font-bold text-gray-700 dark:text-white" for="fotografia">
              Fotografía
            </label>
            <input @change="handleFileUpload"
              class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
              id="fotografia"
              type="file"
            />
          </div>
          <button @click="registerPerson"
            class="bg-gray-700 text-white font-bold py-2 px-4 w-full rounded hover:bg-gray-600"
            type="button">
            Registrar Persona
          </button>
        </form>
        <div v-if="message" :class="{'text-green-500': success, 'text-red-500': !success}" class="text-center mt-4">
          {{ message }}
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios';

// Crea una instancia de Axios
const apiClient = axios.create({
  baseURL: "https://back-end-hospital2-0.onrender.com/",
  headers: {
    Authorization: `Bearer ${localStorage.getItem("token")}`,
  },
});

export default {
  data() {
    return {
      titulo_cortesia: '',
      nombre: '',
      primer_apellido: '',
      segundo_apellido: '',
      curp: '',
      genero: '',
      tipo_sangre: '',
      fecha_nacimiento: '',
      fotografia: null,
      telefono: '',
      correo: '',
      message: '',
      success: false
    };
  },
  methods: {
    handleFileUpload(event) {
      this.fotografia = event.target.files[0];
    },
    async validatePerson() {
      try {
        const response = await apiClient.post('http://127.0.0.1:8000/person/validate', {
          curp: this.curp
        });
        return response.data.exists;
      } catch (error) {
        console.error('Error:', error);
        this.message = 'Ya Existe un Registro con este CURP.';
        this.success = false;
        return false;
      }
    },
    async registerPerson() {
      const exists = await this.validatePerson();
      if (exists) {
        this.message = 'La persona ya está registrada.';
        this.success = false;
        return;
      }
      try {
        const formData = new FormData();
        formData.append('Titulo_Cortesia', this.titulo_cortesia);
        formData.append('Nombre', this.nombre);
        formData.append('Primer_Apellido', this.primer_apellido);
        formData.append('Segundo_Apellido', this.segundo_apellido);
        formData.append('CURP', this.curp);
        formData.append('Genero', this.genero);
        formData.append('Tipo_Sangre', this.tipo_sangre);
        formData.append('Fecha_Nacimiento', this.fecha_nacimiento);
        formData.append('Fotografia', this.fotografia);
        formData.append('Estatus', true);
        formData.append('Fecha_Registro', new Date().toISOString());
        formData.append('Fecha_Actualizacion', new Date().toISOString());

        const response = await apiClient.post('http://127.0.0.1:8000/person/', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        });

        console.log('Persona registrada:', response.data);
        this.message = 'Persona registrada exitosamente.';
        this.success = true;
        this.$emit('person-registered', response.data.id); // Emitir evento con ID de persona registrada
        this.resetForm();
      } catch (error) {
        console.error('Error:', error);
        this.message = 'Ocurrió un error al registrar la persona.';
        this.success = false;
      }
    },
    resetForm() {
      this.titulo_cortesia = '';
      this.nombre = '';
      this.primer_apellido = '';
      this.segundo_apellido = '';
      this.curp = '';
      this.genero = '';
      this.tipo_sangre = '';
      this.fecha_nacimiento = '';
      this.fotografia = null;
      this.telefono = '';
      this.correo = '';
    }
  }
};
</script>
