<template>
  <div class="container-fluid">
    <br />
    <b-row>
      <b-col>
        <h2 class="alert alert-primary text-center uppercase text-">
          Operaciones CRUD de Solicitudes en el Hospital Privilege Care
        </h2>
        <div class="grid grid-cols-1">
          <div class="md:col-span-3">
            <!-- buscador de la tabla   -->
            <nav class="flex items-center justify-between bg-white p-4 shadow-md">
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
              <div v-if="notification.show"
                :class="`flex items-center px-4 py-3 ${notification.type === 'éxito' ? 'bg-red-500' : 'bg-green-500'} text-white text-sm font-bold border rounded-md`"
                role="alert">
                <!-- Debug: Mostrar el tipo de notificación -->
                <p>{{ notification.type }}</p>
                <svg class="fill-current w-4 h-4 mr-2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                  <path v-if="notification.type === 'éxito'"
                    d="M12.432 0c1.34 0 2.01.912 2.01 1.957 0 1.305-1.164 2.512-2.679 2.512-1.269 0-2.009-.75-1.974-1.99C9.789 1.436 10.67 0 12.432 0zM8.309 20c-1.058 0-1.833-.652-1.093-3.524l1.214-5.092c.211-.814.246-1.141 0-1.141-.317 0-1.689.562-2.502 1.117l-.528-.88c2.572-2.186 5.531-3.467 6.801-3.467 1.057 0 1.233 1.273.705 3.23l-1.391 5.352c-.246.945-.141 1.271.106 1.271.317 0 1.357-.392 2.379-1.207l.6.814C12.098 19.02 9.365 20 8.309 20z" />
                  <path v-if="notification.type === 'error'"
                    d="M10 2C5.029 2 1 6.029 1 11s4.029 9 9 9 9-4.029 9-9S14.971 2 10 2zm0 16a7 7 0 1 1 0-14 7 7 0 0 1 0 14zm-.707-7.707a1 1 0 0 0 0-1.414L8.586 8.586a1 1 0 1 0-1.414 1.414L8.293 11l-1.707 1.707a1 1 0 0 0 1.414 1.414L9 12.414l1.707 1.707a1 1 0 0 0 1.414-1.414L10.707 11z" />
                </svg>
                <p>{{ notification.message }}</p>
              </div>

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
              <table class="min-w-full bg-white border border-gray-300 table-hover">
                <thead>
                  <tr class="bg-gray-200 text-gray-600 uppercase text-xs leading-normal">
                    <th class="py-3 px-6 text-left">No°</th>
                    <th class="py-3 px-6 text-left">Personal Médico</th>
                    <th class="py-3 px-6 text-left">N° Solicitud</th>
                    <th class="py-3 px-6 text-left">Comentario</th>
                    <th class="py-3 px-6 text-left">Estatus</th>
                    <th class="py-3 px-6 text-left">Tipo</th>
                    <th class="py-3 px-6 text-left">Fecha de Registro</th>
                    <th class="py-3 px-6 text-left">Fecha de Aprobación</th>
                    <th class="py-3 px-6 text-left">Edita</th>
                    <th class="py-3 px-6 text-left">Elimina</th>
                  </tr>
                </thead>
                <tbody class="text-gray-600 text-xs font-light">
                  <tr v-for="(solicitud, id) in paginatedData" :key="id" class="text-center">
                    <td class="py-3 px-6 text-left whitespace-nowrap truncate">
                      {{ solicitud.id }}
                    </td>
                    <td class="text-center px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                      <div class="flex flex-col items-center text-black-600 font-bold">
                        <h5 class="text-sm">
                          {{ getPersonaNombre(solicitud.Personal_Medico_ID) }}
                        </h5>
                      </div>
                    </td>

                    <td class="text-justify px-6 py-4  text-sm font-medium text-gray-900">

                      <div class="flex flex-col items-center text-back-200">
                        {{ getSolicitud(solicitud.Solicitud_id) }}
                      </div>
                    </td>

                    <td class="text-sm">{{ solicitud.Comentario }}</td>


                    <td>
                      <div v-if="solicitud.Estatus === 'Aprobado'" class="text-center">
                        <a class="iq-icons-list" target="_self">
                          <div class="icon" style="color: green">
                            <i class="fa fa-check"></i>
                          </div>
                          <span style="color: green; font-weight: bold; font-size: 12px;"
                            class="text-sm">Aprobado</span>
                        </a>
                      </div>
                      <div v-else-if="solicitud.Estatus === 'En Proceso'" class="text-center">
                        <a class="iq-icons-list" target="_self">
                          <div class="icon" style="color: #cfae0c">
                            <i class="fa fa-spinner fa-spin"></i>
                          </div>
                          <span style="color: #cfae0c; font-weight: bold;">En Proceso</span>
                        </a>
                      </div>
                      <div v-else-if="solicitud.Estatus === 'Reprogramado'" class="text-center">
                        <a class="iq-icons-list" target="_self">
                          <div class="icon" style="color: #f38b16">
                            <i class="fa fa-retweet" aria-hidden="true"></i>
                          </div>
                          <span style="color: #f38b16; font-weight: bold; font-size: 11px;"
                            class="text-sm">Reprogramado</span>
                        </a>
                      </div>
                      <div v-else-if="solicitud.Estatus === 'Cancelado'" class="text-center">
                        <a class="iq-icons-list" target="_self">
                          <div class="icon" style="color: red">
                            <i class="fa fa-ban"></i>
                          </div>
                          <span style="color: red; font-weight: bold; font-size: 11px;" class="text-sm">Cancelado</span>
                        </a>
                      </div>
                      <div v-else-if="solicitud.Estatus === 'Pausado'" class="text-center">
                        <a class="iq-icons-list" target="_self">
                          <div class="icon" style="color: #3f4152">
                            <i class="fa fa-pause" aria-hidden="true"></i>
                          </div>
                          <span style="color: #3f4152; font-weight: bold; font-size: 12px;"
                            class="text-sm">Pausado</span>
                        </a>
                      </div>
                      <div v-else class="col-12 col-md-6 col-lg-3">
                        {{ solicitud.Estatus }}
                      </div>
                    </td>


                    <td>
                      <div v-if="solicitud.Tipo === 'Servicio Interno'" class="text-center">
                        <a class="iq-icons-list" target="_self">
                          <div class="icon" style="color: brown">
                            <i class="fa fa-h-square" aria-hidden="true"></i>
                          </div>
                          <span style="color: brown; font-weight: bold; font-size: 12px;" class="text-sm">Servicio
                            Interno</span>
                        </a>
                      </div>
                      <div v-else-if="solicitud.Tipo === 'Traslados'" class="text-center">
                        <a class="iq-icons-list" target="_self">
                          <div class="icon" style="color: blue">
                            <i class="fa fa-ambulance" aria-hidden="true"></i>
                          </div>
                          <span style="color: blue; font-weight: bold; font-size: 12px;">Traslados</span>
                        </a>
                      </div>
                      <div v-else-if="solicitud.Tipo === 'Subrogado'" class="text-center">
                        <a class="iq-icons-list" target="_self">
                          <div class="icon" style="color: magenta">
                            <i class="fa fa-user-md" aria-hidden="true"></i>
                          </div>
                          <span style="color: magenta; font-weight: bold; font-size: 12px; "
                            class="text-sm">Subrogado</span>
                        </a>
                      </div>
                      <div v-else-if="solicitud.Tipo === 'Administrativo'" class="text-center">
                        <a class="iq-icons-list" target="_self">
                          <div class="icon" style="color: coffee">
                            <i class="fa fa-hospital" aria-hidden="false"></i>
                          </div>
                          <span style="color: coffee; font-weight: bold; font-size: 12px;"
                            class="text-sm">Administrativo</span>
                        </a>
                      </div>
                      <div v-else class="col-12 col-md-6 col-lg-3">
                        {{ solicitud.Tipo }}
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
                        @click.showModal="editBtn(solicitud.id); showModal = true">
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
                      <a class="iq-icons-list" href="#" target="_self" @click.prevent="deleteSolicitud(solicitud.id)">
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
                {{ currentSolicitud.id ? 'Editar Solicitud' : 'Nueva Solicitud' }}
              </h2>
              <form @submit.prevent="handleSubmit">
                <div class="mb-4">
                  <label for="Personal Medico" class="block text-gray-700">Personal Médico</label>
                  <select class="form-control" v-model="currentSolicitud.Personal_Medico_ID">
                    <option v-for="persona in personas" :key="persona.id" :value="persona.id">
                      {{ persona.Titulo_Cortesia }} {{ persona.Nombre }} {{ persona.Primer_Apellido }} {{
                      persona.Segundo_Apellido }}
                    </option>
                  </select>
                </div>

                <div class="mb-4">
                  <label for="Solicitud" class="block text-gray-700">Solicitud</label>
                  <select class="form-control" v-model="currentSolicitud.Solicitud_id">
                    <option v-for="rol in roles" :key="rol.ID" :value="rol.ID">
                      {{ rol.ID }} {{ rol.Prioridad }}
                    </option>
                  </select>
                </div>


                <div class="mb-4">
                  <label for="Descripcion" class="block text-gray-700">Comentario</label>
                  <textarea id="Descripcion" v-model="currentSolicitud.Comentario"
                    class="w-full border border-gray-300 rounded p-2" required rows="4"></textarea>
                </div>

                <div class="mb-4">
                  <label for="Estatus" class="form-label float-left ml-2">Estatus</label>
                  <select class="form-control" v-model="currentSolicitud.Estatus">
                    <option value="Aprobado">Aprobado</option>
                    <option value="En Proceso">En Proceso</option>
                    <option value="Reprogramado">Reprogramado</option>
                    <option value="Pausado">Pausado</option>
                    <option value="Cancelado">Cancelado</option>
                  </select>
                </div>

                <div class="mb-4">
                  <label for="Tipo" class="form-label float-left ml-2">Tipo</label>
                  <select class="form-control" v-model="currentSolicitud.Tipo">
                    <option value="Servicio Interno">Servicio Interno</option>
                    <option value="Traslados">Traslados</option>
                    <option value="Subrogado">Subrogado</option>
                    <option value="Administrativo">Administrativo</option>
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
                    {{ currentSolicitud.id ? 'Actualizar' : 'Crear' }}
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


const apiClient = axios.create({
  baseURL: 'https://back-end-hospital2-0.onrender.com/',
  headers: {
    'Authorization': `Bearer ${localStorage.getItem('token')}`
  }
});

export default {

  name: "AprobacionesModal",
  data() {
    return {
      solicitudes: [],
      currentSolicitud: {},
      api: "https://back-end-hospital2-0.onrender.com/tbb_aprobaciones/",
      solicitud: {

        Personal_Medico_ID: "",
        Solicitud_id: "",
        Fecha_Registro: "",
        Estatus: "",
        Tipo: "",
        Comentario: "",
        Fecha_Actualizacion: "",
      },

      showModal: false,
      searchInput: "",
      currentPage: 1, // Página actual
      resultsPerPage: 10, // Resultados por página
      totalPages: 1, // Total de páginas
      paginatedData: [], // Datos de la página actual

      config: {
        dateFormat: "Y-m-d",
        inline: true,
      },




      dataTable: [],  // Datos de la tabla
      personas: [],    // Datos del personal médico
      solicitudes: [],    // Datos del personal médico


      notification: {
        show: false,
        message: '',
        type: '' // "success" o "error"
      }


    };
  },

  mounted() {
    body[0].classList.add("sidebar-main-menu");
    console.log("DOM is rendered");
    console.log(Object.keys(this.currentSolicitud).length);
 

    this.fetchData();

    this.fetchData1()

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
        if (this.currentSolicitud.id) {
          await apiClient.put(`${this.api}${this.currentSolicitud.id}/`, this.currentSolicitud);
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

    async getSolicitudes() {
      try {
        const response = await apiClient.get(this.api);
        this.solicitudes = response.data;
        this.updatePagination();
      } catch (error) {
        console.error('Error fetching solicitudes:', error);
      }
    },

    saveSolicitud() {
      // Verifica si no se ha insertado ninguna fecha y hora en Fecha_Actualizacion
      if (!this.solicitud.Fecha_Actualizacion) {
        this.solicitud.Fecha_Actualizacion = null; // Establece Fecha_Actualizacion como null
      }

      // Asegúrate de que todos los campos requeridos estén presentes
      const valid = this.solicitud.Personal_Medico_ID &&
        this.solicitud.Solicitud_id &&
        this.solicitud.Fecha_Registro &&
        this.solicitud.Estatus &&
        this.solicitud.Tipo &&
        this.solicitud.Comentario;

      if (!valid) {
        console.error('Todos los campos requeridos deben estar completos.');
        return;
      }

      apiClient
        .post(this.api, this.solicitud)
        .then((response) => {
          console.log('Aprobación creada:', response.data);
          this.getSolicitudes();
          this.solicitud = {}; // Limpia el formulario
        })
        .catch((error) => {
          console.error('Error al crear la aprobación:', error.response ? error.response.data : error.message);
        });
    },

    editBtn(id) {
      console.log(id); // Verifica que `id` no sea `undefined`
      this.solicitudes.map((solicitud) => {
        if (solicitud.id === id) {
          console.log(solicitud.Personal_Medico_ID);
          this.currentSolicitud = {
            id: solicitud.id,
            Personal_Medico_ID: solicitud.Personal_Medico_ID,
            Solicitud_id: solicitud.Solicitud_id,
            Fecha_Registro: solicitud.Fecha_Registro,
            Estatus: solicitud.Estatus,
            Tipo: solicitud.Tipo,
            Comentario: solicitud.Comentario,
            Fecha_Actualizacion: solicitud.Fecha_Actualizacion
              ? solicitud.Fecha_Actualizacion
              : null,
          };
        }
      });
    },


    updateSolicitud(id) {
      if (!this.currentSolicitud.Fecha_Actualizacion) {
        this.currentSolicitud.Fecha_Actualizacion = null;
      }
      apiClient
        .put(`${this.api}${id}/`, this.currentSolicitud) // Asegúrate de que `id` sea correcto
        .then((response) => {
          console.log(response.data);
          this.getSolicitudes();
          this.currentSolicitud = {};
        })
        .catch((error) => {
          console.log(error);
        });
    },

    deleteSolicitud(id) {
      const solicitud = this.solicitudes.find(
        (solicitud) => solicitud.id === id
      );

      if (
        confirm(`¿Deseas eliminar la solicitud? 
               - ID: ${solicitud.id}
               - Servicio: ${solicitud.Personal_Medico_ID}
               - Departamento solicitante: ${solicitud.Solicitud_id}
               - Fecha de Solicitud: ${this.formatearFecha(solicitud.Fecha_Registro)}
               - Estatus: ${solicitud.Estatus}
               - Tipo: ${solicitud.Tipo}
               - Comentario: ${solicitud.Comentario}
               - Fecha de Aprobación: ${this.formatearFecha(solicitud.Fecha_Actualizacion)}`)
      ) {
        apiClient
          .delete(`${this.api}${id}/`) // Usa la URL correcta
          .then((response) => {
            console.log(response.data);
            this.getSolicitudes();
          })
          .catch((error) => {
            console.log(error);
          });
      }
      this.showNotification('Exito', 'Solicitud eliminada.');
    },




    // Actualiza la paginación basada en la búsqueda y el número total de solicitudes
    updatePagination() {
      console.log("Actualizando la paginación...");

      const searchQuery = this.searchInput.toLowerCase();

      const filteredData = this.solicitudes.filter((solicitud) => {
        // Convertimos todos los campos a cadena y a minúsculas para comparación
        const personaID = String(solicitud.Persona_ID || '').toLowerCase();
        const solicitudID = String(solicitud.Solicitud || '').toLowerCase();
        const comentario = String(solicitud.Comentario || '').toLowerCase();
        const estatus = String(solicitud.Estatus || '').toLowerCase();
        const tipo = String(solicitud.Tipo || '').toLowerCase();
        const fecha = String(solicitud.Fecha || '').toLowerCase();

        // Comprobamos si el query de búsqueda coincide con alguno de los campos
        return (
          personaID.includes(searchQuery) ||
          solicitudID.includes(searchQuery) ||
          comentario.includes(searchQuery) ||
          estatus.includes(searchQuery) ||
          tipo.includes(searchQuery) ||
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
        const [tableDataResponse, personasResponse] = await Promise.all([
          fetch('https://back-end-hospital2-0.onrender.com/tbb_aprobaciones/'),
          fetch('https://back-end-hospital2-0.onrender.com/persons/'),

        ]);
        this.dataTable = await tableDataResponse.json();
        this.personas = await personasResponse.json();

        console.log('DataTable:', this.dataTable);
        console.log('Personas:', this.personas);

      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },

    getPersonaNombre(personalId) {
      console.log('Buscando nombre para ID:', personalId);
      // Convierte ambos IDs a número para la comparación
      const persona = this.personas.find(p => Number(p.id) === Number(personalId));
      if (persona) {
        return `${persona.Titulo_Cortesia} ${persona.Nombre} ${persona.Primer_Apellido} ${persona.Segundo_Apellido}`;
      }
      return 'Nombre no disponible';
    },



    async fetchData1() {
      try {
        const [tableDataResponse, rolesResponse] = await Promise.all([
          apiClient.get('tbb_aprobaciones/'),
          apiClient.get('tbd_solicitudes/')
        ]);
        this.dataTable1 = tableDataResponse.data;
        this.roles = rolesResponse.data;

        console.log('DataTable:', this.dataTable1);
        console.log('Roles:', this.roles);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },


    getSolicitud(solicitudId) {
      console.log('Buscando el Rol:', solicitudId);

      // Verifica que `roles` está definido y es un objeto
      if (!this.roles || typeof this.roles !== 'object') {
        return 'Datos de la Solicitud no disponible';
      }

      // Convierte `roles` en un arreglo de valores y busca la solicitud
      const rolesArray = Object.values(this.roles);
      const soli = rolesArray.find(p => Number(p.ID) === Number(solicitudId));

      if (soli) {
        const id = String(soli.ID || '');
        const prioridad = String(soli.Prioridad || '');
        const descripcion = String(soli.Descripcion || '');
        const estatus = String(soli.Estatus || '');

        return `Solicitud:${id} Prioridad:${prioridad}  Descripción: ${descripcion}\nSolicitud: ${estatus}`;
      }

      return 'Datos de la Solicitud no disponible';
    }

  },


  watch: {
    // Observa los cambios en el input de búsqueda
    searchInput() {
      this.updatePagination();
    }

  },




  computed: {
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
        switch (solicitud.Personal_Medico_ID) {
          case 1:
            servicioText = "Urgencias";
            break;

          default:
            servicioText = String(solicitud.Nombre, solicitud.Descripcion); // Si no coincide con ninguno, mantener el valor original
        }

        const matchServicioId = servicioText
          .toLowerCase()
          .includes(this.searchInput.toLowerCase());

        let departamentoText = "";
        switch (solicitud.Solicitud_id) {
          case 1:
            departamentoText = "Dirección General";
            break;

          default:
            departamentoText = String(solicitud.Solicitud_id);
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
