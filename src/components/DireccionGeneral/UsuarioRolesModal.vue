<template>
  <div class="container-fluid">
    <br />
    <b-row>
      <b-col>
        <div class="grid grid-cols-1">
          <div class="md:col-span-3">
            <h2 class="alert alert-primary text-center">
              Operaciones CRUD de Lista de Usuarios y Roles
            </h2>
            <!-- buscador de la tabla -->
            <nav
              class="flex items-center justify-between bg-white p-4 shadow-md"
            >
            <div class="flex items-center">
                <div class="">
                  <form action="#" class="flex items-center w-full">
                    <input type="text"
                      class="border border-gray-300 rounded-full pl-4 pr-12 py-2 bg-gray-50 focus:outline-none focus:border-blue-500 w-full placeholder-gray-500"
                      title="searchField" placeholder="Buscar" v-model="searchInput" />
                    <button class="absolute right-0 mr-3 text-gray-500 hover:text-gray-700">
                      <i class="ri-search-line"></i>
                    </button>
                  </form>
                </div>
              </div>

                            <!-- Notificación -->
                            <!-- <div v-if="notification.show"
                            :class="`flex items-center px-4 py-3 ${notification.type === 'éxito' ? 'bg-red-500' : 'bg-green-500'} text-white text-sm font-bold border rounded-md`"
                            role="alert"> -->
                            <!-- Debug: Mostrar el tipo de notificación -->
                            <!-- <p>{{ notification.type }}</p>
                            <svg class="fill-current w-4 h-4 mr-2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                              <path v-if="notification.type === 'éxito'"
                                d="M12.432 0c1.34 0 2.01.912 2.01 1.957 0 1.305-1.164 2.512-2.679 2.512-1.269 0-2.009-.75-1.974-1.99C9.789 1.436 10.67 0 12.432 0zM8.309 20c-1.058 0-1.833-.652-1.093-3.524l1.214-5.092c.211-.814.246-1.141 0-1.141-.317 0-1.689.562-2.502 1.117l-.528-.88c2.572-2.186 5.531-3.467 6.801-3.467 1.057 0 1.233 1.273.705 3.23l-1.391 5.352c-.246.945-.141 1.271.106 1.271.317 0 1.357-.392 2.379-1.207l.6.814C12.098 19.02 9.365 20 8.309 20z" />
                              <path v-if="notification.type === 'error'"
                                d="M10 2C5.029 2 1 6.029 1 11s4.029 9 9 9 9-4.029 9-9S14.971 2 10 2zm0 16a7 7 0 1 1 0-14 7 7 0 0 1 0 14zm-.707-7.707a1 1 0 0 0 0-1.414L8.586 8.586a1 1 0 1 0-1.414 1.414L8.293 11l-1.707 1.707a1 1 0 0 0 1.414 1.414L9 12.414l1.707 1.707a1 1 0 0 0 1.414-1.414L10.707 11z" />
                            </svg>
                            <p>{{ notification.message }}</p>
                          </div> -->
              

              <div>
                <button @click="showModal = true" class="flex items-center text-blue-500 hover:text-blue-700">
                  <svg class="h-10 w-10" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"
                    stroke-linecap="round" stroke-linejoin="round">
                    <rect x="3" y="3" width="18" height="18" rx="2" ry="2" />
                    <line x1="12" y1="8" x2="12" y2="16" />
                    <line x1="8" y1="12" x2="16" y2="12" />
                  </svg>
                  <h5 class="ml-2 uppercase font-bold">crear</h5> <!-- Añadimos un margen a la izquierda -->
                </button>
              </div>


            </nav>

            <div class="overflow-x-auto">
              <table
                class="min-w-full bg-white border border-gray-300 table-hover"
              >
                <thead>
                  <tr
                    class="bg-gray-200 text-gray-600 uppercase text-xs leading-normal"
                  >
                    <th class="py-3 px-6 text-left">No°</th>
                    <th class="py-3 px-6 text-left">Usuario</th>
                    <th class="py-3 px-6 text-left">Rol Asignado</th>
                    <th class="py-3 px-6 text-left">Estatus</th>
                    <th class="py-3 px-6 text-left">Fecha de Registro</th>
                    <th class="py-3 px-6 text-left">Fecha de Actualización</th>
                    <th class="py-3 px-6 text-left">Edita</th>
                    <th class="py-3 px-6 text-left">Elimina</th>
                  </tr>
                </thead>
                <tbody class="text-gray-600 text-xs font-light">
                  <tr
                    v-for="(solicitud, Usuario_ID) in paginatedData"
                    :key="Usuario_ID"
                    class="text-center"
                  >
                    <td class="py-3 px-6 text-left whitespace-nowrap truncate">
                      {{ solicitud.Usuario_ID }}
                    </td>

                    <td
                      class="text-center px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
                      :style="{ whiteSpace: 'pre-line' }"
                    >
                      <div class="flex flex-col items-centertext-black-600 font-bold">
                        <h5 class="text-sm">
                          {{ getPersonaNombre(solicitud.Usuario_ID) }}
                        </h5>
                      </div>
                    </td>

                    <td
                      class="text-center px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
                    >
                    <div class="flex flex-col items-center text-green-600">
                      <h5 class="text-sm">
                        {{ getRolNombre(solicitud.Rol_ID) }}
                      </h5>
                      <div v-if="getRolNombre(solicitud.Rol_ID) === 'Medico General'">
                        <!-- SVG para Medico General -->
                          <img src="" alt="">
                      </div>
                      <div v-else-if="getRolNombre(solicitud.Rol_ID) === 'Medico Especialista'">
                        <!-- SVG para Medico Especialista -->
                        <svg width="10px" height="10px" viewBox="0 0 16 16" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
                          <path fill="#444" d="M14 11.3c-1-1.9-2-1.6-3.1-1.7 0.1 0.3 0.1 0.6 0.1 1 1.6 0.4 2 2.3 2 3.4v1h-2v-1h1c0 0 0-2.5-1.5-2.5s-1.5 2.4-1.5 2.5h1v1h-2v-1c0-1.1 0.4-3.1 2-3.4 0-0.6-0.1-1.1-0.2-1.3-0.2-0.1-0.4-0.3-0.4-0.6 0-0.6 0.8-0.4 1.4-1.5 0 0 0.9-2.3 0.6-4.3h-1c0-0.2 0.1-0.3 0.1-0.5s0-0.3-0.1-0.5h0.8c-0.3-1-1.3-1.9-3.2-1.9 0 0 0 0 0 0s0 0 0 0 0 0 0 0c-1.9 0-2.9 0.9-3.3 2h0.8c0 0.2-0.1 0.3-0.1 0.5s0 0.3 0.1 0.5h-1c-0.2 2 0.6 4.3 0.6 4.3 0.6 1 1.4 0.8 1.4 1.5 0 0.5-0.5 0.7-1.1 0.8-0.2 0.2-0.4 0.6-0.4 1.4 0 0.4 0 0.8 0 1.2 0.6 0.2 1 0.8 1 1.4 0 0.7-0.7 1.4-1.5 1.4s-1.5-0.7-1.5-1.5c0-0.7 0.4-1.2 1-1.4 0-0.3 0-0.7 0-1.2s0.1-0.9 0.2-1.3c-0.7 0.1-1.5 0.4-2.2 1.7-0.6 1.1-0.9 4.7-0.9 4.7h13.7c0.1 0-0.2-3.6-0.8-4.7zM6.5 2.5c0-0.8 0.7-1.5 1.5-1.5s1.5 0.7 1.5 1.5-0.7 1.5-1.5 1.5-1.5-0.7-1.5-1.5z"></path>
                          <path fill="#444" d="M5 13.5c0 0.276-0.224 0.5-0.5 0.5s-0.5-0.224-0.5-0.5c0-0.276 0.224-0.5 0.5-0.5s0.5 0.224 0.5 0.5z"></path>
                          </svg>
                      </div>
                      <div v-else-if="getRolNombre(solicitud.Rol_ID) === 'Direccion General'">
                        <!-- SVG para Direccion General -->
                        <svg width="24" height="24" xmlns="http://www.w3.org/2000/svg">
                          <!-- Contenido del SVG -->
                        </svg>
                      </div>
                      <div v-else-if="getRolNombre(solicitud.Rol_ID) === 'Enfermero'">
                        <!-- SVG para Enfermero -->
                        <svg width="24" height="24" xmlns="http://www.w3.org/2000/svg">
                          <!-- Contenido del SVG -->
                        </svg>
                      </div>
                      <div v-else-if="getRolNombre(solicitud.Rol_ID) === 'Paciente'">
                        <!-- SVG para Paciente -->
                        <svg height="800px" width="800px" version="1.1" id="_x32_" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" 
                                  viewBox="0 0 512 512"  xml:space="preserve">
                              <g>
                                <path class="st0" d="M256,265.308c73.252,0,132.644-59.391,132.644-132.654C388.644,59.412,329.252,0,256,0
                                  c-73.262,0-132.643,59.412-132.643,132.654C123.357,205.917,182.738,265.308,256,265.308z"/>
                                <path class="st0" d="M425.874,393.104c-5.922-35.474-36-84.509-57.552-107.465c-5.829-6.212-15.948-3.628-19.504-1.427
                                  c-27.04,16.672-58.782,26.399-92.819,26.399c-34.036,0-65.778-9.727-92.818-26.399c-3.555-2.201-13.675-4.785-19.505,1.427
                                  c-21.55,22.956-51.628,71.991-57.551,107.465C71.573,480.444,164.877,512,256,512C347.123,512,440.427,480.444,425.874,393.104z"/>
                              </g>
                        </svg> 
                      </div>
                    </div>
                    </td>

                    <td>
                      <div v-if="solicitud.Estatus" class="text-center">
                        <a class="iq-icons-list" href="#" target="_self">
                          <div class="icon" style="color: green">
                            <i class="fa fa-check"></i>
                          </div>
                          <span style="color: green" class="text-sm"
                            >Activo</span
                          >
                        </a>
                      </div>
                      <div v-else class="text-center">
                        <a class="iq-icons-list" href="#" target="_self">
                          <div class="icon" style="color: orange">
                            <i class="fa fa-spinner fa-spin"></i>
                          </div>
                          <span style="color: orange">Inactivo</span>
                        </a>
                      </div>
                    </td>

                    <td class="text-sm">
                      {{ formatearFecha(solicitud.Fecha_Registro) }}
                    </td>

                    <td class="text-sm">
                      {{ formatearFecha(solicitud.Fecha_Actualizacion) }}
                    </td>


                    <td class="text-center px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                      <a class="iq-icons-list " href="#" target="_self"
                        @click.showModal="editBtn(solicitud.Usuario_ID, solicitud.Rol_ID); showModal = true">
                        <div class="icon" style="color: blue; font-size: 25px; ">
                          <svg class="h-7 w-7 text-blue-500 hover:text-blue-900" viewBox="0 0 24 24" stroke-width="2"
                            stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
                            <path stroke="none" d="M0 0h24v24H0z" />
                            <path d="M9 7 h-3a2 2 0 0 0 -2 2v9a2 2 0 0 0 2 2h9a2 2 0 0 0 2 -2v-3" />
                            <path d="M9 15h3l8.5 -8.5a1.5 1.5 0 0 0 -3 -3l-8.5 8.5v3" />
                            <line x1="16" y1="5" x2="19" y2="8" />
                          </svg>
                        </div>
                      </a>
                    </td>

                    <td class="text-center px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                      <a class="iq-icons-list" href="#" target="_self" @click.prevent="deleteSolicitud(solicitud.Usuario_ID, solicitud.Rol_ID)">
                        <div class="icon" style="color: red; font-size: 25px; text-align: center;">
                          <svg class="h-7 w-7 text-red-500 hover:text-red-900" width="24" height="24"
                            viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none"
                            stroke-linecap="round" stroke-linejoin="round">
                            <path stroke="none" d="M0 0h24v24H0z" />
                            <line x1="4" y1="7" x2="20" y2="7" />
                            <line x1="10" y1="11" x2="10" y2="17" />
                            <line x1="14" y1="11" x2="14" y2="17" />
                            <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />
                            <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" />
                          </svg>
                        </div>
                      </a>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>




                    <!-- Modal -->
          <div v-if="showModal" class="fixed inset-0 flex items-center justify-center bg-gray-900 bg-opacity-50">
            <div class="bg-white p-6 rounded shadow-lg w-full max-w-md relative">
              <button @click="showModal = false, this.currentSolicitud = {}"
                class="absolute top-3 right-3 text-gray-600 hover:text-gray-900">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" viewBox="0 0 20 20" fill="currentColor"
                  aria-hidden="true">
                  <path fill-rule="evenodd"
                    d="M10 9.293L14.707 4.586a1 1 0 10-1.414-1.414L10 7.465 5.707 3.293a1 1 0 00-1.414 1.414L8.586 9.293 4.293 13.586a1 1 0 101.414 1.414L10 10.707l4.293 4.293a1 1 0 001.414-1.414L10 9.293z"
                    clip-rule="evenodd" />
                </svg>
              </button>
              <h2 class="text-lg font-semibold text-center mb-4">
                {{ currentSolicitud.Usuario_ID ? 'Editar Solicitud' : 'Nueva Solicitud' }}
              </h2>
              <form @submit.prevent="handleSubmit">
                <div class="mb-4">
                  <label for="Personal Medico" class="block text-gray-700">Usuario</label>
                  <select class="form-control" v-model="currentSolicitud.Usuario_ID">
                    <option                           v-for="usuario in usuarios"
                    :key="usuario.Persona_ID"
                    :value="usuario.Persona_ID"
                  >
                  {{ usuario.Nombre_Usuario }}
                    </option>
                  </select>
                </div>

                <div class="mb-4">
                  <label for="Solicitud" class="block text-gray-700">Rol</label>
                  <select                         class="form-control"
                  v-model="currentSolicitud.Rol_ID"
                  required>
                  <option
                  v-for="rol in roles"
                  :key="rol.id"
                  :value="rol.id"
                >
                  {{ rol.Nombre }}
                </option>
                  </select>
                </div>


                <div class="mb-4">
                  <label for="Descripcion" class="block text-gray-700">Estado</label>
                  <select
                  class="form-control"
                  v-model="currentSolicitud.Estatus"
                  required
                >
                  <option :value="true">Activo</option>
                  <option :value="false">Inactivo</option>
                </select>
                </div>



                <div class="mb-4">
                  <label for="Fecha_Registro" class="block text-gray-700">Fecha de Registro</label>
                  <input type="datetime-local" id="Fecha_Registro" v-model="currentSolicitud.Fecha_Registro"
                    class="w-full border border-gray-300 rounded p-2" required />
                </div>

                <div class="mb-4">
                  <label for="Fecha_Actualizacion" class="block text-gray-700">Fecha de Actualización</label>
                  <input type="datetime-local" id="Fecha_Actualizacion" v-model="currentSolicitud.Fecha_Actualizacion"
                    class="w-full border border-gray-300 rounded p-2" required />
                </div>
                <div class="text-center">
                  <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
                    {{ currentSolicitud.Usuario_ID ? 'Actualizar' : 'Crear' }}
                  </button>
                </div>
              </form>
            </div>
          </div>


        </div>
      </b-col>
    </b-row>

    
    <br>
    <!-- Pagination Controls -->
    <div class="flex justify-between items-center mt-2">
      <button @click="previousPage" :disabled="currentPage === 1"
        class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-700">
        Anterior
      </button>
      <span class="text-gray-700">Página {{ currentPage }} de {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages"
        class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-700">
        Siguiente
      </button>
    </div>
    <br>
  </div>


</template>

<style>
.hover-zoom {
  transition: transform 0.3s;
}

.hover-zoom:hover {
  transform: scale(1.05);
}
</style>

<script>
import axios from "axios";

import moment from "moment";

const body = document.getElementsByTagName("body");

// Crea una instancia de Axios
const apiClient = axios.create({
  baseURL: "https://back-end-hospital2-0.onrender.com/",
  headers: {
    Authorization: `Bearer ${localStorage.getItem("token")}`,
  },
});

export default {
  name: "AprobacionServiciosHospitalarios",
  data() {
    return {
      solicitudes: [],
      currentSolicitud: {},
      api: "https://back-end-hospital2-0.onrender.com/usuario_roles/",
      solicitud: {
        Usuario_ID: "",
        Rol_ID: "",
        Estatus: "",
        Fecha_Registro: "",
        Fecha_Actualizacion: "",
      },
    
      showModal: false,
      searchInput: "",
      currentPage: 1, // Página actual
      resultsPerPage: 10, // Resultados por página
      paginatedData: [], // Datos de la página actual

      config: {
        dateFormat: "Y-m-d",
        inline: true,
      },

      dataTable: [], // Datos de la tabla
      personas: [], // Datos del personal médico
      usuarios: [], // Datos de los usuarios
      dataTable1: [], // Datos de la tabla
      roles: [], // Datos del personal médico
    };
  },

  mounted() {
    body[0].classList.add("sidebar-main-menu");
    console.log("DOM is rendered");
    console.log(Object.keys(this.currentSolicitud).length);

    this.fetchData();
    this.fetchData1();
    this.getSolicitudes();
  },

  unmounted() {
    body[0].classList.remove("sidebar-main-menu");
  },

  created() {
    console.log("DOM is created");
    this.getSolicitudes();
  },

  methods: {

    showNotification(message, type = 'info') {
      this.notification.message = message;
      this.notification.type = type;
      this.notification.show = true;

      // Ocultar notificación después de 5 segundos
      setTimeout(() => {
        this.notification.show = false;
      }, 5000);
    },


    async handleSubmit() {
  try {
    if (this.currentSolicitud.Usuario_ID && this.currentSolicitud.Rol_ID) {
      // Construir correctamente la URL concatenando los IDs
      const url = `${this.api}${this.currentSolicitud.Usuario_ID}/${this.currentSolicitud.Rol_ID}/`;
      await apiClient.put(url, this.currentSolicitud);
    } else {
      await apiClient.post(this.api, this.currentSolicitud);
    }
    this.getSolicitudes(); // Actualiza la lista de solicitudes
    this.currentSolicitud = {}; // Limpia el formulario
    this.showModal = false; // Cierra el modal
    this.showNotification('éxito', 'Agregado');
  } catch (error) {
    console.error('Error al Agregar:', error);
  }
},




    getSolicitudes() {
      apiClient
        .get("usuario_roles/")
        .then((response) => {
          console.log(response.data);
          this.solicitudes = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    saveSolicitud() {
      if (!this.solicitud.Fecha_Actualizacion) {
        this.solicitud.Fecha_Actualizacion = null;
      }

      const valid =
        this.solicitud.Usuario_ID &&
        this.solicitud.Rol_ID &&
        this.solicitud.Estatus &&
        this.solicitud.Fecha_Registro;

      if (!valid) {
        console.error("Todos los campos requeridos deben estar completos.");
        return;
      }

      apiClient
        .post("usuario_roles/", this.solicitud)
        .then((response) => {
          console.log("Aprobación creada:", response.data);
          this.getSolicitudes();
          this.solicitud = {}; // Limpia el formulario
        })
        .catch((error) => {
          console.error(
            "Error al crear la aprobación:",
            error.response ? error.response.data : error.message
          );
        });
    },

    editBtn(Usuario_ID, Rol_ID) {
      console.log(Usuario_ID, Rol_ID); // Verifica que ambos IDs están definidos
      this.solicitudes.map((solicitud) => {
        if (
          solicitud.Usuario_ID === Usuario_ID &&
          solicitud.Rol_ID === Rol_ID
        ) {
          console.log(solicitud);
          this.currentSolicitud = {
            Usuario_ID: solicitud.Usuario_ID,
            Rol_ID: solicitud.Rol_ID,
            Estatus: solicitud.Estatus,
            Fecha_Registro: solicitud.Fecha_Registro,
            Fecha_Actualizacion: solicitud.Fecha_Actualizacion
              ? new Date(solicitud.Fecha_Actualizacion).toISOString()
              : null,
          };
        }
      });
    },

    updateSolicitud() {
      const { Usuario_ID, Rol_ID } = this.currentSolicitud;

      if (!Usuario_ID || !Rol_ID) {
        console.error(
          "Usuario_ID o Rol_ID no están definidos en currentSolicitud"
        );
        return;
      }

      if (!this.currentSolicitud.Fecha_Actualizacion) {
        this.currentSolicitud.Fecha_Actualizacion = new Date().toISOString();
      }

      const updatedSolicitud = {
        Usuario_ID: this.currentSolicitud.Usuario_ID,
        Rol_ID: this.currentSolicitud.Rol_ID,
        Estatus: this.currentSolicitud.Estatus,
        Fecha_Actualizacion: this.currentSolicitud.Fecha_Actualizacion,
      };

      console.log(
        `Actualizando solicitud con Usuario_ID: ${Usuario_ID} y Rol_ID: ${Rol_ID}`
      );

      apiClient
        .put(`usuario_roles/${Usuario_ID}/${Rol_ID}/`, updatedSolicitud)
        .then((response) => {
          console.log(response.data);
          this.getSolicitudes(); // Actualiza la lista de solicitudes
          this.currentSolicitud = {}; // Reinicia el formulario si es necesario
        })
        .catch((error) => {
          console.error(
            "Error al actualizar la solicitud:",
            error.response ? error.response.data : error.message
          );
        });
    },

    deleteSolicitud(Usuario_ID, Rol_ID) {
      const solicitud = this.solicitudes.find(
        (solicitud) =>
          solicitud.Usuario_ID === Usuario_ID && solicitud.Rol_ID === Rol_ID
      );

      if (
        confirm(`¿Deseas eliminar la solicitud? 
                  - Usuario: ${solicitud.Usuario_ID}
                  - Rol: ${solicitud.Rol_ID}
                  - Estatus: ${solicitud.Estatus}
                  - Fecha de Solicitud: ${this.formatearFecha(
                    solicitud.Fecha_Registro
                  )}
                  - Fecha de Aprobación: ${this.formatearFecha(
                    solicitud.Fecha_Actualizacion
                  )}`)
      ) {
        apiClient
          .delete(`usuario_roles/${Usuario_ID}/${Rol_ID}/`)
          .then((response) => {
            console.log(response.data);
            this.getSolicitudes();
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },



 // Actualiza la paginación basada en la búsqueda y el número total de solicitudes
    updatePagination() {
      console.log("Actualizando la paginación...");

      const searchQuery = this.searchInput.toLowerCase();

      const filteredData = this.solicitudes.filter((solicitud) => {
        // Convertimos todos los campos a cadena y a minúsculas para comparación
        const Usuario_ID = String(solicitud.Usuario_ID || '').toLowerCase();
        const Rol_ID = String(solicitud.Rol_ID || '').toLowerCase();
        const Estatus = String(solicitud.Estatus || '').toLowerCase();
        const fecha = String(solicitud.Fecha_Actualizacion || '').toLowerCase();

        // Comprobamos si el query de búsqueda coincide con alguno de los campos
        return (
          Usuario_ID.includes(searchQuery) ||
          Rol_ID.includes(searchQuery) ||
          Estatus.includes(searchQuery) ||
          fecha.includes(searchQuery)
        );
      });

      this.totalPages = Math.ceil(filteredData.length / this.resultsPerPage);

      this.paginatedData = filteredData.slice(
        (this.currentPage - 1) * this.resultsPerPage,
        this.currentPage * this.resultsPerPage
      );
    },
    // Cambia a la página anterior
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },

    goToPage(page) {
      if (page >= 1 && page <= this.totalPages) {
        this.currentPage = page;
        console.log(`Ir a la página: ${this.currentPage}`);
        this.updatePagination();
      }
    },




    formatearFecha(fecha) {
      if (moment(fecha, moment.ISO_8601, true).isValid()) {
        return moment(fecha).format("DD/MM/YYYY HH:mm:ss");
      } else {
        return "Sin Fecha";
      }
    },

    async fetchData() {
      try {
        const [tableDataResponse, personasResponse, usuariosResponse] =
          await Promise.all([
            apiClient.get("tbb_aprobaciones/"),
            apiClient.get("persons/"),
            apiClient.get("tbb_usuarios/"),
          ]);
        this.dataTable = tableDataResponse.data;
        this.personas = personasResponse.data;
        this.usuarios = usuariosResponse.data;

        console.log("DataTable:", this.dataTable);
        console.log("Personas:", this.personas);
        console.log("Usuarios:", this.usuarios);
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },

    async fetchData1() {
      try {
        const [tableDataResponse, rolesResponse] = await Promise.all([
          apiClient.get("usuario_roles/"),
          apiClient.get("roles/"),
        ]);
        this.dataTable1 = tableDataResponse.data;
        this.roles = rolesResponse.data;

        console.log("DataTable:", this.dataTable1);
        console.log("Roles:", this.roles);
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },

    getUsuarioNombre(usuarioId) {
      console.log("Buscando nombre para ID de usuario:", usuarioId);
      const usuario = this.usuarios.find(
        (u) => Number(u.ID) === Number(usuarioId)
      );
      if (usuario) {
        return `${usuario.Nombre_Usuario}`;
      }
      return "Nombre no disponible";
    },

    getPersonaNombre(personalId) {
      console.log("Buscando nombre para ID:", personalId);
      const persona = this.personas.find(
        (p) => Number(p.id) === Number(personalId)
      );
      const usuario = this.usuarios.find(
        (u) => Number(u.Persona_ID) === Number(personalId)
      );

      if (persona) {
        const usuarioAsignado = usuario
          ? usuario.Nombre_Usuario
          : "Usuario no Encontrado";
        return `Nombre: ${persona.Titulo_Cortesia} ${persona.Nombre} ${persona.Primer_Apellido} ${persona.Segundo_Apellido}\nUsuario: ${usuarioAsignado}`;
      }
      return "Nombre no disponible";
    },

    getRolNombre(rolId) {
      console.log("Buscando el Rol:", rolId);
      const rol = this.roles.find((p) => Number(p.id) === Number(rolId));
      if (rol) {
        return `${rol.Nombre}`;
      }
      return "Rol no disponible";
    },
  },

  async fetchData1() {
    try {
      const [tableDataResponse, rolesResponse] = await Promise.all([
        fetch("https://back-end-hospital2-0.onrender.com/usuario_roles/"),
        fetch("https://back-end-hospital2-0.onrender.com/roles/"),
      ]);
      this.dataTable1 = await tableDataResponse.json();
      this.roles = await rolesResponse.json();

      console.log("DataTable:", this.dataTable1);
      console.log("Roles:", this.roles);
    } catch (error) {
      console.error("Error fetching data:", error);
    }
  },

  getRolNombre(rolId) {
    console.log("Buscando el Rol:", rolId);
    // Convierte ambos IDs a número para la comparación
    const rol = this.roles.find((p) => Number(p.id) === Number(rolId));
    if (rol) {
      return `${rol.Nombre}`;
    }
    return "Rol no disponible";
  },


  watch: {
    // Observa los cambios en el input de búsqueda
    searchInput() {
      this.updatePagination();
    }

  },

  computed: {
    // esta en adaptacion a la plantilla
 // esta en adaptacion a la plantilla

 paginatedItems() {
      // Lógica para obtener los elementos paginados
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.items.slice(start, end);
    },


    paginatedData() {
      const startIndex = (this.currentPage - 1) * this.resultsPerPage;
      const endIndex = startIndex + this.resultsPerPage;
      return this.filteredData
        .slice(startIndex, endIndex)
        .map((item, index) => {
          return {
            ...item,
            index: startIndex + index + 1, // Ajusta el índice para mantener la secuencia numérica continua
          };
        });
    },

    totalPages() {
      return Math.ceil(this.solicitudes.length / this.resultsPerPage);
    },

    filteredData() {
      // Modifica la función para filtrar según el término de búsqueda
      console.log("Search input:", this.searchInput);
      return this.solicitudes.filter((solicitud) => {
        const matchSearchInput = Object.values(solicitud).some((value) => {
          return String(value)
            .toLowerCase()
            .includes(this.searchInput.toLowerCase());
        });

        // Obtener el texto correspondiente al servicio_paciente_id
        let servicioText = "";
        switch (solicitud.Usuario_ID) {
          case 1:
            servicioText = "Urgencias";
            break;

          default:
            servicioText = String(solicitud.Usuario_ID); // Si no coincide con ninguno, mantener el valor original
        }

        const matchServicioId = servicioText
          .toLowerCase()
          .includes(this.searchInput.toLowerCase());

        let departamentoText = "";
        switch (solicitud.Usuario_ID) {
          case 1:
            departamentoText = "Dirección General";
            break;

          default:
            departamentoText = String(solicitud.Usuario_ID);
        }

        const matchDepartamentoId = departamentoText
          .toLowerCase()
          .includes(this.searchInput.toLowerCase());

        return matchSearchInput || matchServicioId || matchDepartamentoId;
      });
    },
  },
};
</script>

<style scoped>
.usuario-nombre {
  display: block;
  margin-top: 5px;
  font-size: 0.9em;
  color: #555;
}
</style>
