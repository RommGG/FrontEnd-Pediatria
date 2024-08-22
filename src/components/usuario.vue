<template>
  <div class="py-4 md:py-8">
    <div class="flex flex-col lg:flex-row bg-white rounded-lg shadow-lg overflow-hidden mx-auto max-w-sm lg:max-w-2xl">
      <!-- Columna de imagen -->
      <div
        class="w-full h-48 lg:h-auto bg-gray-400 dark:bg-gray-800 hidden lg:block lg:w-5/12 bg-cover rounded-t-lg lg:rounded-l-lg"
        style="background-image: url('https://i.pinimg.com/originals/ec/0e/69/ec0e690e32f7c84e714886136a2f6469.jpg');"
      ></div>
      <!-- Columna de formulario -->
      <div class="w-full lg:w-7/12 bg-white dark:bg-gray-700 p-4 lg:p-6 rounded-lg lg:rounded-l-none">
        <h3 class="text-xl lg:text-2xl text-center text-gray-800 dark:text-white mb-4 lg:mb-6">Registro Usuario</h3>
        <form @submit.prevent="registerUser" class="space-y-4 lg:space-y-6">
          <!-- Usuario -->
          <div>
            <label class="block mb-1 text-sm font-bold text-gray-700 dark:text-white" for="usuario">Usuario</label>
            <input
              v-model="username"
              class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
              id="usuario"
              type="text"
              placeholder="Usuario"
              required
            />
          </div>
          <!-- Correo Electrónico -->
          <div>
            <label class="block mb-1 text-sm font-bold text-gray-700 dark:text-white" for="correo">Correo Electrónico</label>
            <input
              v-model="email"
              class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
              id="correo"
              type="email"
              placeholder="Correo Electrónico"
              required
            />
          </div>
          <!-- Número Telefónico Móvil -->
          <div>
            <label class="block mb-1 text-sm font-bold text-gray-700 dark:text-white" for="telefono">Número Telefónico Móvil</label>
            <input
              v-model="phoneNumber"
              class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
              id="telefono"
              type="tel"
              placeholder="Número Telefónico Móvil"
              required
            />
          </div>
          <!-- Contraseña -->
          <div>
            <label class="block mb-1 text-sm font-bold text-gray-700 dark:text-white" for="password">Contraseña</label>
            <input
              v-model="password"
              class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
              id="password"
              type="password"
              placeholder="******"
              required
            />
          </div>
          <!-- Repetir Contraseña -->
          <div>
            <label class="block mb-1 text-sm font-bold text-gray-700 dark:text-white" for="repetirContrasena">Repetir Contraseña</label>
            <input
              v-model="repeatPassword"
              class="w-full px-3 py-2 text-sm leading-tight text-gray-700 dark:text-white border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
              id="repetirContrasena"
              type="password"
              placeholder="******"
              required
            />
            <p v-if="password && repeatPassword && password !== repeatPassword" class="text-red-500 text-xs italic mt-1">Las contraseñas no coinciden.</p>
          </div>
          <!-- Botón de Registro -->
          <div>
            <button
              class="bg-gray-700 text-white font-bold py-2 px-4 w-full rounded hover:bg-gray-600"
              type="submit"
            >
              Registrar Usuario
            </button>
          </div>
          <!-- Mensajes -->
          <div v-if="successMessage || errorMessage" class="text-center">
            <p v-if="successMessage" class="text-green-500 text-xs italic mb-2">{{ successMessage }}</p>
            <p v-if="errorMessage" class="text-red-500 text-xs italic mb-2">{{ errorMessage }}</p>
            <button v-if="successMessage" class="bg-gray-700 text-white font-bold py-2 px-4 w-full rounded hover:bg-gray-600 mt-2" @click="login">
              Iniciar
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>



<script>
import axios from 'axios';

export default {
  data() {
    return {
      username: '',
      email: '',
      phoneNumber: '',
      password: '',
      repeatPassword: '',
      errorMessage: '',
      successMessage: '',
      personaID: null // Asegúrate de asignar esto correctamente
    };
  },
  methods: {

    goToLogin() {
      // Redirigir a la ruta de login
      this.$router.push('/login');
    },
    handleSuccess() {
      this.successMessage = 'Usuario registrado con éxito.';
    },

    async fetchPersonaID() {
      try {
        const response = await axios.get('https://back-end-hospital2-0.onrender.com/persons/');
        const personas = response.data;
        console.log('Datos de personas:', personas);
        if (personas.length > 0) {
          this.personaID = personas[personas.length - 1].id + 0;
        } else {
          this.personaID = 0;
        }
      } catch (error) {
        console.error('Error al obtener Persona_ID:', error);
      }
    },
    async registerUser() {
      if (this.password !== this.repeatPassword) {
        this.errorMessage = 'Las contraseñas no coinciden.';
        return;
      }

      if (!Number.isInteger(this.personaID)) {
        this.errorMessage = 'Persona_ID debe ser un entero válido.';
        return;
      }

      const user = {
        Persona_ID: this.personaID,
        Nombre_Usuario: this.username,
        Correo_Electronico: this.email,
        Numero_Telefonico_Movil: this.phoneNumber,
        Contrasena: this.password,
        Estatus: 'Activo',
        Fecha_Registro: new Date().toISOString(),
        Fecha_Actualizacion: new Date().toISOString()
      };

      try {
        const response = await axios.post('https://back-end-hospital2-0.onrender.com/tbb_usuarios/', user);
        console.log('Usuario registrado:', response.data);
        this.successMessage = 'Usuario registrado con éxito.';
        this.errorMessage = '';
        this.resetForm();
      } catch (error) {
        console.error('Error al registrar usuario:', error.response ? error.response.data : error);
        this.errorMessage = error.response ? error.response.data.detail : 'Error al registrar usuario.';
        this.successMessage = '';
      }
    },
    resetForm() {
      this.username = '';
      this.email = '';
      this.phoneNumber = '';
      this.password = '';
      this.repeatPassword = '';
    }
  },
  created() {
    this.fetchPersonaID();
  }
};
</script>

<style scoped>
.text-red-500 {
  color: #f56565;
}
.text-green-500 {
  color: #48bb78;
}
.text-xs {
  font-size: 0.75rem;
}
.italic {
  font-style: italic;
}
.mt-4 {
  margin-top: 1rem;
}
</style>
