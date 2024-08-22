<template>
  <div class="container-fluid">
    <br />
    <b-row>
      <b-col>
        <h2 class="alert alert-primary text-center uppercase text-">
          Operaciones CRUD Asignacion de Roles en el Hospital Privilege Care
        </h2>
        <div class="grid grid-cols-1">
          
          <div class="md:col-span-4">

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
              <table class="min-w-full bg-white border border-gray-300 table-fixed">
                <thead>
                  <tr class="bg-gray-200 text-gray-600 uppercase text-xs leading-normal text-center">
                    <th class="py-3 px-6 text-left">No°</th>
                    <th class="py-3 px-6 text-left">Nombre</th>
                    <th class="py-3 px-6 text-left">Descripción</th>
                    <th class="py-3 px-6 text-left">Estatus</th>
                    <th class="py-3 px-6 text-left">Fecha de Registro</th>
                    <th class="py-3 px-6 text-left">Fecha de Actualización</th>
                    <th class="py-3 px-6 text-left">Edita</th>
                    <th class="py-3 px-6 text-left">Elimina</th>
                  </tr>
                </thead>
                <tbody class="text-gray-600 text-xs font-light">

                  <tr v-for="(solicitud, id) in paginatedData" :key="id" class="text-center">
                    <td class="py-3 px-6 text-left whitespace-nowrap truncate">
                      {{ solicitud.id }}
                    </td>

                    <td class="text-center px-6 py-8 whitespace-nowrap  text-md-center font-medium text-gray-900">
                      <div class="flex flex-col text-black-700 text-blod ">
                        <h5 class="text-sm">
                          {{ solicitud.Nombre }}
                        </h5>
                      </div>
                    </td>

                    <td class="text-justify px-8 py-4 text-gray-500 font-medium">
                      {{ solicitud.Descripcion }}</td>
                    <td>


                      <div v-if="solicitud.Estatus" class="text-center">
                        <a class="iq-icons-list" href="#" target="_self">
                          <div class="icon" style="color: green">
                            <i class="fa fa-check"></i>
                          </div>
                          <span style="color: green" class="text-sm">Activo</span>
                        </a>
                      </div>
                      <div v-else class="text-center">
                        <a class="iq-icons-list" href="#" target="_self">
                          <div class="icon" style="color: #FF5F3F">
                            <i class="fa fa-pause-circle" aria-hidden="true"></i>
                          </div>
                          <span style="color: #FF5F3F">Inactivo</span>
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
                  <label for="Nombre" class="block text-gray-700">Nombre</label>
                  <input type="text" id="Nombre" v-model="currentSolicitud.Nombre"
                    class="w-full border border-gray-300 rounded p-2" required />
                </div>
                <div class="mb-4">
                  <label for="Descripcion" class="block text-gray-700">Descripción</label>
                  <textarea id="Descripcion" v-model="currentSolicitud.Descripcion"
                    class="w-full border border-gray-300 rounded p-2" required rows="4"></textarea>
                </div>
                <div class="mb-4">
                  <label for="Estatus" class="block text-gray-700">Estatus</label>
                  <select id="Estatus" v-model="currentSolicitud.Estatus"
                    class="w-full border border-gray-300 rounded p-2" required>
                    <option :value="true" style="color: green;" >Activo</option>
                    <option :value="false" style="color: #FF5F3F;">Inactivo</option>
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
                    class="w-full border border-gray-300 rounded p-2"  />
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



<div class="container py-3">
  <div class="row">
    <div class="col-md-4 mb-4">
      <div class="bg-white shadow rounded overflow-hidden p-2 hover-zoom">
        <div class="rounded text-center">
          <img
            src="@/assets/img/imagenes-direccionGeneral/usuariosMedicos.jpg"
            alt="Estructura Organica Hospitalaria"
            class="mx-auto d-block"
            style="width: 200px; height: auto; border-radius: 5px; padding-bottom: 5%;"
          />
        </div>
        <div class="text-center">
          <button
            class="btn btn-primary"
            onclick="window.location.href='/usuarioRoles'"
          >
          Usuarios y Roles
          </button>
        </div>
      </div>
    </div>

    <div class="col-md-4 mb-4">
      <div class="bg-white shadow rounded overflow-hidden p-2 hover-zoom">
        <div class="rounded text-center">
          <img
            src="@/assets/img/imagenes-direccionGeneral/Aprobaciones.jpg"
            alt="Estructura Organica Hospitalaria"
            class="mx-auto d-block"
            style="width: 300px; height: auto; border-radius: 5px; padding-bottom: 5%;"
          />

        </div>
        <div class="text-center">
          <button
            class="btn btn-primary"
            onclick="window.location.href='/AprobacionSM'"
          >
            Aprobaciones
          </button>
        </div>
      </div>
    </div>

    <div class="col-md-4 mb-4">
      <div class="bg-white shadow rounded overflow-hidden p-2 hover-zoom">
        <div class="rounded text-center">
          <img
            src="@/assets/img/imagenes-direccionGeneral/bitacora.jpg"
            alt="Estructura Organica Hospitalaria"
            class="mx-auto d-block"
            style="width: 320px; height: auto; border-radius: 5px; padding-bottom: 5%;"
          />
        </div>
        <div class="text-center">
          <button
            class="btn btn-primary"
            onclick="window.location.href='/Bitacora'"
          >
            Bitácora
          </button>
        </div>
      </div>
    </div>

    <div class="col-md-4 mb-4">
      <div class="bg-white shadow rounded overflow-hidden p-2 hover-zoom">
        <div class="rounded text-center">
          <img
            src="@/assets/img/imagenes-direccionGeneral/Aprobaciones.jpg"
            alt="Estructura Organica Hospitalaria"
            class="mx-auto d-block"
            style="width: 300px; height: auto; border-radius: 5px; padding-bottom: 5%;"
          />

        </div>
        <div class="text-center">
          <button
            class="btn btn-primary"
            onclick="window.location.href='/RolesModal'"
          >
            Roles Modal
          </button>
        </div>
      </div>
    </div>

    
  </div>
</div>




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

  name: "RolesModal",
  data() {
    return {

      solicitudes: [],
      currentSolicitud: {},
      api: "https://back-end-hospital2-0.onrender.com/roles/",
      solicitud: {
        Nombre: "",
        Descripcion: "",
        Estatus: "",
        Fecha_Registro: "",
        Fecha_Actualizacion: "",
      },

      showModal: false,
      searchInput: "",
      currentPage: 1, // Página actual
      resultsPerPage: 5, // Resultados por página
      totalPages: 1, // Total de páginas
      paginatedData: [], // Datos de la página actual

      config: {
        dateFormat: "Y-m-d",
        inline: true,
      },

    };
  },

  mounted() {
    body[0].classList.add("sidebar-main-menu");
    console.log("DOM is rendered");
    console.log(Object.keys(this.currentSolicitud).length);


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
      } catch (error) {
        console.error('Error saving solicitud:', error);
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

      // Verifica los campos requeridos y crea un mensaje específico para cada uno
      let missingFields = [];
      if (!this.solicitud.Nombre) {
        missingFields.push('Nombre');
      }
      if (!this.solicitud.Descripcion) {
        missingFields.push('Descripción');
      }
      if (!this.solicitud.Estatus) {
        missingFields.push('Estatus');
      }
      if (!this.solicitud.Fecha_Registro) {
        missingFields.push('Fecha de Registro');
      }

      // Si hay campos faltantes, muestra un mensaje de advertencia
      if (missingFields.length > 0) {
        this.showWarningMessage = true;
        console.error(`Faltan los siguientes campos: ${missingFields.join(', ')}.`);
        return;
      }

      // Oculta el mensaje de advertencia si todos los datos están presentes
      this.showWarningMessage = false;

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
            Nombre: solicitud.Nombre,
            Descripcion: solicitud.Descripcion,
            Estatus: solicitud.Estatus,
            Fecha_Registro: solicitud.Fecha_Registro,
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
        .put(`${this.api}${id}/`, this.currentSolicitud)
        .then((response) => {
          console.log(response.data);
          this.getSolicitudes();
          this.currentSolicitud = {};
          // Recargar la página
          window.location.reload();
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
                 - Nombre: ${solicitud.Nombre}
                 - Descripción: ${solicitud.Descripcion}
                 - Estatus: ${solicitud.Estatus}
                 - Fecha de Solicitud: ${this.formatearFecha(solicitud.Fecha_Registro)}
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
    },





    

  // Actualiza la paginación basada en la búsqueda y el número total de solicitudes
  updatePagination() {

    console.log("Actualizando la paginación...");

      const filteredData = this.solicitudes.filter((solicitud) =>
        solicitud.Nombre.toLowerCase().includes(this.searchInput.toLowerCase())
      );

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
  },

watch:{
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



