<template>
  <div class="py-16">
    <div
      class="flex bg-white rounded-lg shadow-lg overflow-hidden mx-auto max-w-sm lg:max-w-4xl"
    >
      <div
        class="hidden lg:block lg:w-1/2 bg-cover"
        style="
          background-image: url('https://media.revistagq.com/photos/5ca5fd6e3492a940f5bf1bf0/1:1/w_511,h_511,c_limit/doctor_mike_gq_5080.jpg');
        "
      ></div>
      <div class="w-full p-8 lg:w-1/2">
        <h2 class="text-2xl font-semibold text-gray-700 text-center">
          PRIVILAGE CARE
        </h2>
        <p class="text-xl text-gray-600 text-center">HOSPITAL</p>
        <a
          href="#"
          class="flex items-center justify-center mt-4 text-white rounded-lg shadow-md hover:bg-gray-100"
        >
          <div class="px-4 py-3">
            <svg class="h-6 w-6" viewBox="0 0 40 40">
              <!-- SVG paths for Google icon -->
            </svg>
          </div>
          <h1 class="px-4 py-3 w-5/6 text-center text-gray-600 font-bold">
            Inicia Sesión con Google
          </h1>
        </a>
        <div class="mt-4 flex items-center justify-between">
          <span class="border-b w-1/5 lg:w-1/4"></span>
          <a href="#" class="text-xs text-center text-gray-500 uppercase"
            >o Inicia Sesión con tu email</a
          >
          <span class="border-b w-1/5 lg:w-1/4"></span>
        </div>
        <form @submit.prevent="login">
          <div class="mt-4">
            <label class="block text-gray-700 text-sm font-bold mb-2"
              >Rol</label
            >
            <select
              v-model="selectedRole"
              @change="fetchRole"
              class="bg-gray-200 text-gray-700 focus:outline-none focus:shadow-outline border border-gray-300 rounded py-2 px-4 block w-full appearance-none"
              required
            >
              <option v-for="role in roles" :key="role.id" :value="role.id">
                {{ role.Nombre }}
              </option>
            </select>
          </div>
          <div class="mt-4">
            <label class="block text-gray-700 text-sm font-bold mb-2"
              >Email</label
            >
            <input
              v-model="usuario.Correo_Electronico"
              class="bg-gray-200 text-gray-700 focus:outline-none focus:shadow-outline border border-gray-300 rounded py-2 px-4 block w-full appearance-none"
              type="email"
              required
            />
          </div>
          <div class="mt-4">
            <div class="flex justify-between">
              <label class="block text-gray-700 text-sm font-bold mb-2"
                >Contraseña</label
              >
              <a href="#" class="text-xs text-gray-500"
                >¿Olvidaste tu Contraseña?</a
              >
            </div>
            <input
              v-model="usuario.Contrasena"
              class="bg-gray-200 text-gray-700 focus:outline-none focus:shadow-outline border border-gray-300 rounded py-2 px-4 block w-full appearance-none"
              type="password"
              required
            />
          </div>
          <div class="mt-8">
            <button
              type="submit"
              class="bg-gray-700 text-white font-bold py-2 px-4 w-full rounded hover:bg-gray-600"
            >
              Iniciar Sesión
            </button>
          </div>
        </form>
        <div class="mt-4 flex items-center justify-between">
          <span class="border-b w-1/5 md:w-1/4"></span>
          <RouterLink to="/register">
            <a href="#" class="text-xs text-gray-500 uppercase"
              >o ¡Regístrate!</a
            >
          </RouterLink>
          <span class="border-b w-1/5 md:w-1/4"></span>
        </div>
        <p v-if="mensaje" class="mt-4 text-center text-red-500">
          {{ mensaje }}
        </p>
        <p v-if="token" class="mt-4 text-center text-green-500">
          Token: {{ token }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { RouterLink } from "vue-router";

const apiClient = axios.create({
  baseURL: "https://back-end-hospital2-0.onrender.com/",
  headers: {
    Authorization: `Bearer ${localStorage.getItem("token")}`,
  },
});

export default {
  data() {
    return {
      usuario: {
        Nombre_Usuario: "", // Puede estar vacío si no se usa
        Correo_Electronico: "",
        Numero_Telefonico_Movil: "", // Puede estar vacío si no se usa
        Contrasena: "",
      },
      roles: [], // Lista de roles desde el backend
      selectedRole: null, // Rol seleccionado
      mensaje: "",
      token: "", // Añadido para almacenar el token
    };
  },
  components: {
    RouterLink,
  },
  async created() {
    try {
      const response = await apiClient.get("https://back-end-hospital2-0.onrender.com/roles/");
      this.roles = response.data; // Guarda la lista de roles en el estado
    } catch (error) {
      this.mensaje = "Error al cargar los roles";
    }
  },
  methods: {
    async login() {
      try {
        const response = await apiClient.post("https://back-end-hospital2-0.onrender.com/login/", {
          Nombre_Usuario: this.usuario.Nombre_Usuario,
          Correo_Electronico: this.usuario.Correo_Electronico,
          Numero_Telefonico_Movil: this.usuario.Numero_Telefonico_Movil,
          Contrasena: this.usuario.Contrasena,
          Rol: this.selectedRole, // Incluye el rol seleccionado en la solicitud
        });

        this.token = response.data.token; // Almacena el token en el estado
        localStorage.setItem("token", this.token);

        // Guarda el rol y el nombre de usuario en el localStorage
        localStorage.setItem("role", this.selectedRole);
        localStorage.setItem("correo", this.usuario.Correo_Electronico);

        this.$router.push("/dashboard");
      } catch (error) {
        if (error.response) {
          this.mensaje = error.response.data.detail || "Acceso Denegado";
        } else {
          this.mensaje = "Error de conexión";
        }
      }
    },

    async fetchRole() {
      try {
        const response = await apiClient.get(
          `https://back-end-hospital2-0.onrender.com/roles/${this.selectedRole}`
        );
        // Procesa la respuesta si es necesario
      } catch (error) {
        if (error.response) {
          console.error("Error de respuesta:", error.response.data);
          this.mensaje = `Error al obtener el rol: ${
            error.response.data.detail || "Error desconocido"
          }`;
        } else if (error.request) {
          console.error("Error en la solicitud:", error.request);
          this.mensaje = "Error en la solicitud: El servidor no respondió";
        } else {
          console.error("Error:", error.message);
          this.mensaje = "Error: " + error.message;
        }
      }
    },
  },
};
</script>

<style scoped>
/* Estilos adicionales aquí */
</style>
