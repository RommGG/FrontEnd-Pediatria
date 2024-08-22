<template>
  <div class="container-fluid">
    <h1 style="text-align: center">
      Operaciones CRUD Asignacion de Roles en el Hospital Privilege Care
    </h1>
    <br />
    <b-row>
      <b-col>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-12">
          <div class="md:col-span-3">
            <h2 class="alert alert-primary text-center">
              Lista de Roles
            </h2>
            <!-- buscador de la tabla   -->
            <nav
              class="flex items-center justify-between bg-white p-4 shadow-md"
            >
              <div class="flex items-center">
                <div class="relative w-full lg:w-100">
                  <form action="#" class="flex items-center w-full">
                    <input
                      type="text"
                      class="border border-gray-300 rounded-full pl-4 pr-12 py-2 bg-gray-50 focus:outline-none focus:border-blue-500 w-full placeholder-gray-500"
                      title="searchField"
                      placeholder="Buscar"
                      v-model="searchInput"
                    />
                    <button
                      class="absolute right-0 mr-3 text-gray-500 hover:text-gray-700"
                    >
                      <i class="ri-search-line"></i>
                    </button>
                  </form>
                </div>
              </div>
              <div class="flex items-center">
                <button
                  @click="miniSidebar"
                  class="flex items-center justify-center w-10 h-10 rounded-full hover:bg-gray-100 focus:outline-none"
                >
                  <i class="ri-more-fill"></i>
                </button>
                <button
                  class="ml-4 lg:hidden focus:outline-none"
                  @click="toggleNav"
                >
                  <i class="ri-menu-3-line text-xl"></i>
                </button>
                <div class="hidden lg:flex items-center ml-4">
                  <slot name="responsiveRight" />
                </div>
                <div class="hidden lg:flex items-center ml-4">
                  <slot name="right" />
                </div>
              </div>
            </nav>

            <div class="overflow-x-auto">
              <table class="min-w-full bg-white border border-gray-300 table-hover">
                <thead>
                  <tr class="bg-gray-200 text-gray-600 uppercase text-xs leading-normal">
                    <th class="py-3 px-6 text-left">No°</th>
                    <th class="py-3 px-6 text-left">Nombre</th>
                    <th class="py-3 px-6 text-left">Descripción</th>
                    <th class="py-3 px-6 text-left">Estatus</th>
                    <th class="py-3 px-6 text-left">Fecha de Registro</th>
                    <th class="py-3 px-6 text-left">Fecha de Actualizacióni</th>
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
                      <div class="flex flex-col  text-green-600">
                        <h5 class="text-sm">
                          {{ solicitud.Nombre }}
                        </h5>
                      </div>
                    </td>

                    <td class="text-sm">{{ solicitud.Descripcion }}</td>


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


                    <td>

                      <a class="iq-icons-list" href="#" target="_self" @click.prevent="editBtn(solicitud.id)">
                        <div class="icon" style="color: blue; font-size: 25px;">
                          <svg class="h-8 w-8 text-blue-500"  viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">  <path stroke="none" d="M0 0h24v24H0z"/>  <path d="M9 7 h-3a2 2 0 0 0 -2 2v9a2 2 0 0 0 2 2h9a2 2 0 0 0 2 -2v-3" />  <path d="M9 15h3l8.5 -8.5a1.5 1.5 0 0 0 -3 -3l-8.5 8.5v3" />  <line x1="16" y1="5" x2="19" y2="8" /></svg>                          </div>
                      </a>

                    </td>
                    <td>
                      <a class="iq-icons-list" href="#" target="_self" @click.prevent="deleteSolicitud(solicitud.id)">
                        <div class="icon" style="color: red; font-size: 25px;">
                          <svg class="h-8 w-8 text-red-500"  width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">  <path stroke="none" d="M0 0h24v24H0z"/>  <line x1="4" y1="7" x2="20" y2="7" />  <line x1="10" y1="11" x2="10" y2="17" />  <line x1="14" y1="11" x2="14" y2="17" />  <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />  <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" /></svg>
                        </div>
                      </a>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

          <!-- Edicion de la Solicitudes -->
          <div class="col-md-11">
            <div v-if="Object.keys(this.currentSolicitud).length !== 0">
              <h2 class="alert alert-warning" role="alert" style="text-align: center;">
                Actualiza Rol
              </h2>

              <form @submit.prevent="updateSolicitud(currentSolicitud.id)">
                <div class="row">

                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2" scope="col"
                        >Nombre</label
                      >
                      <input
                      class="form-control"
                      v-model="currentSolicitud.Nombre"
                      >
                    </div>
                  </div>
                </div>

                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Descripción</label>
                      <textarea
                        class="form-control"
                        v-model="currentSolicitud.Descripcion"
                        rows="5"
                        cols="50"
                      ></textarea>
                    </div>
                  </div>
                </div>



                <div class="col">
                  <div class="form-group">
                    <label class="form-label float-left ml-2">Estado</label>
                    <select
                      class="form-control"
                      v-model="currentSolicitud.Estatus"
                    >
                    <option :value="true">Activo</option>
                    <option :value="false">Inactivo</option>
                    </select>
                  </div>
                </div>

                

                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2"
                        >Fecha de Registro</label
                      >
                      <input
                        type="text"
                        class="form-control"
                        v-model="currentSolicitud.Fecha_Registro"
                      />
                    </div>
                  </div>
                </div>


                <div class="col">
                  <div class="form-group">
                    <label class="form-label float-left ml-2"
                      >Fecha de Aprobacion</label
                    >
                    <input
                      type="datetime"
                      class="form-control"
                      v-model="currentSolicitud.Fecha_Actualizacion"
                    />
                  </div>
                </div>
                <br />

                <button type="submit" class="btn btn-success float-left ml-2">
                  Actualzar
                </button>
              </form>
            </div>

            <!-- Crear Solicitud  -->

            <div v-else>
              <h2 class="alert alert-success" style="text-align: center;" >Crea Rol</h2>

              <form @submit.prevent="saveSolicitud()">

                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Nombre del Rol</label>
                      <input
                      class="form-control"
                      v-model="solicitud.Nombre"
                      required
                    >
                    </div>
                  </div>
                </div>

                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Descripción</label>
                      <textarea
                        class="form-control"
                        v-model="solicitud.Descripcion"
                        required
                        rows="5"
                        cols="50"
                      ></textarea>
                    </div>
                  </div>
                </div>


                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Estatus</label>
                      <select
                        class="form-control"
                        v-model="solicitud.Estatus"
                        required
                      >
                      <option :value="true">Activo</option>
                      <option :value="false">Inactivo</option>
                      </select>
                    </div>
                  </div>
                </div>

                
                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2"
                        >Fecha de Registro</label
                      >
                      <input
                        type="datetime-local"
                        class="form-control"
                        v-model="solicitud.Fecha_Registro"
                        required
                      />
                    </div>
                  </div>
                </div>


                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2"
                        >Fecha de Aprobacion</label
                      >
                      <input
                        type="datetime-local"
                        class="form-control"
                        v-model="solicitud.Fecha_Actualizacion"
                      />
                    </div>
                  </div>
                </div>
                <br />

                
                <button type="submit" class="btn btn-primary float-left ml-2">
                  Guardar
                </button>
              </form>
            </div>
          </div>
        </div>
      </b-col>
    </b-row>

    <div class="pagination">
      <button
        class="btn btn-primary"
        @click="previousPage"
        :disabled="currentPage === 1"
      >
        Anterior
      </button>

      <button
        class="btn btn-primary"
        @click="nextPage"
        :disabled="currentPage === totalPages"
      >
        Siguiente
      </button>
    </div>
    <br />
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
      api: "https://back-end-hospital2-0.onrender.com/roles/",
      solicitud: {

        Nombre: "",
        Descripcion: "",
        Estatus: "",
        Fecha_Registro: "",
        Fecha_Actualizacion: "",
      },
      searchInput: "",
      currentPage: 1, // Página actual
      resultsPerPage: 10, // Resultados por página

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



  },

  unmounted() {
    body[0].classList.remove("sidebar-main-menu");
  },

  created() {
    console.log("DOM is created");
    this.getSolicitudes();
 
  },

  methods: {
    getSolicitudes() {
      apiClient
        .get(this.api)
        .then((response) => {
          console.log(response.data);
          this.solicitudes = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    saveSolicitud() {
    // Verifica si no se ha insertado ninguna fecha y hora en Fecha_Actualizacion
    if (!this.solicitud.Fecha_Actualizacion) {
      this.solicitud.Fecha_Actualizacion = null; // Establece Fecha_Actualizacion como null
    }

    // Asegúrate de que todos los campos requeridos estén presentes
    const valid = this.solicitud.Nombre && 
                  this.solicitud.Descripcion &&
                  this.solicitud.Estatus && 
                  this.solicitud.Fecha_Registro ;

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


    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },

    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
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

  computed: {
    // esta en adaptacion a la plantilla

    paginatedData() {
      const startIndex = (this.currentPage - 1) * this.resultsPerPage;
      const endIndex = startIndex + this.resultsPerPage;
      return this.filteredData
        .slice(startIndex, endIndex)
        .map((item, index) => {
          return {
            ...item,
            index: startIndex + index + 1, // Ajustar el índice para mantener la secuencia numérica continua
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

        // console.log("Entrada de búsqueda de coincidencias:", matchSearchInput);
        // console.log(
        //   "Entrada de búsqueda de servicio_paciente_id:",
        //   matchServicioId
        // );
        // console.log(
        //   "Entrada de búsqueda de departamento_solicitante:",
        //   matchDepartamentoId
        // );

        return matchSearchInput || matchServicioId || matchDepartamentoId;
      });
    },






  },
};
</script>
