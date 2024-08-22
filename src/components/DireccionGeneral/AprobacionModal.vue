
  <template>
    <div class="container-fluid">
      <h1 style="text-align: center">
        Operaciones CRUD de Solicitudes de Procedimientos Clínicos y Quirúrgicos
      </h1>
      <br />
      <b-row>
        <b-col>
          <div class="grid grid-cols-1 md:grid-cols-4 gap-12">
            <div class="md:col-span-3">
              <h2 class="alert alert-primary text-center">
                Lista de Solicitudes
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
                      <th class="py-3 px-6 text-left">Personal Médico</th>
                      <th class="py-3 px-6 text-left">N° Solicitud</th>
                      <th class="py-3 px-6 text-left">Comentario</th>
                      <th class="py-3 px-6 text-left">Estatus</th>
                      <th class="py-3 px-6 text-left">Tipo</th>
                      <th class="py-3 px-6 text-left">Fecha de Registro</th>
                      <th class="py-3 px-6 text-left">Fecha de Aprobación</th>
                      <th class="py-3 px-6 text-left">Acciones</th>
                    </tr>
                  </thead>
                  <tbody class="text-gray-600 text-xs font-light">
                    <tr v-for="(solicitud, id) in paginatedData" :key="id" class="text-center">
                      <td class="py-3 px-6 text-left whitespace-nowrap truncate">
                        {{ solicitud.id }}
                      </td>
                      <td class="text-center px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                        <div class="flex flex-col items-center text-green-600">
                          <h5 class="text-sm">
                            {{ getPersonaNombre(solicitud.Personal_Medico_ID) }}
                          </h5>
                        </div>
                      </td>
  
                      <td class="text-justify px-6 py-4  text-sm font-medium text-gray-900">
  
                        <div class="col-12 col-md-6 col-lg-3">
                          {{getSolicitud(solicitud.Solicitud_id) }}
                        </div>
                      </td>
  
                      <td class="text-sm">{{ solicitud.Comentario }}</td>
  
  
                      <td>
                        <div v-if="solicitud.Estatus === 'Aprobado'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: green">
                              <i class="fa fa-check"></i>
                            </div>
                            <span style="color: green" class="text-sm">Aprobado</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Estatus === 'En Proceso'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: orange">
                              <i class="fa fa-spinner fa-spin"></i>
                            </div>
                            <span style="color: orange">En Proceso</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Estatus === 'Reprogramado'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: red">
                              <i class="fa fa-times"></i>
                            </div>
                            <span style="color: red" class="text-sm">Reprogramado'</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Estatus === 'Cancelado'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: black">
                              <i class="fa fa-ban"></i>
                            </div>
                            <span style="color: black" class="text-sm">Cancelado</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Estatus === 'Pausado'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: black">
                              <i class="fa fa-ban"></i>
                            </div>
                            <span style="color: black" class="text-sm">Pausado</span>
                          </a>
                        </div>
                        <div v-else class="col-12 col-md-6 col-lg-3">
                          {{ solicitud.Estatus }}
                        </div>
                      </td>
  
  
                      <td>
                        <div v-if="solicitud.Tipo === 'Servicio Interno'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: brown">
                              <i class="fa fa-h-square" aria-hidden="true"></i>
                            </div>
                            <span style="color: brown" class="text-sm">Servicio Interno</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Tipo === 'Traslados'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: blue">
                              <i class="fa fa-ambulance" aria-hidden="true"></i>
                            </div>
                            <span style="color: blue">Traslados</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Tipo === 'Subrogado'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: magenta">
                              <i class="fa fa-user-md" aria-hidden="true"></i>
                            </div>
                            <span style="color: magenta" class="text-sm">Subrogado</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Tipo === 'Administrativo'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: coffee">
                              <i class="fa fa-hospital" aria-hidden="false"></i>
                            </div>
                            <span style="color: coffee" class="text-sm">Administrativo</span>
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
                      <td>
                        <a href="#" class="edit" title="">
                          <button class="btn btn-warning btn-sm text-sm" @click.prevent="editBtn(solicitud.id)">
                            Edita
                          </button>
                        </a>
                        <a href="#" class="edit" title="">
                          <button class="btn btn-danger btn-sm" @click.prevent="deleteSolicitud(solicitud.id)">
                            Elimina
                          </button>
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
                <h2 class="alert alert-warning" role="alert">
                  Actualiza Solicitud
                </h2>
  
                <form @submit.prevent="updateSolicitud(currentSolicitud.id)">
                  <div class="row">
                    <div class="col">
                      <div class="form-group">
                        <label class="form-label float-left ml-2" scope="col"
                          >Personal Médico</label
                        >
                        <select
                        class="form-control"
                        v-model="currentSolicitud.Personal_Medico_ID"
                      >
                        <option v-for="persona in personas" :key="persona.id" :value="persona.id">
                          {{ persona.Titulo_Cortesia }} {{ persona.Nombre }} {{ persona.Primer_Apellido }} {{ persona.Segundo_Apellido }}
                        </option>
                      </select>
                      </div>
                    </div>
                  </div>
  
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2"
                        >Solicitud</label
                      >
                      <select
                        class="form-control"
                        v-model="currentSolicitud.Solicitud_id"
                      >
                      <option v-for="rol in roles" :key="rol.ID" :value="rol.ID">
                      {{ rol.ID}} {{ rol.Prioridad }} 
                      </option>
                      </select>
                    </div>
                  </div>
                                
  
                  <div class="row">
                    <div class="col">
                      <div class="form-group">
                        <label class="form-label float-left ml-2"
                          >Comentario</label
                        >
                        <input
                          type="text"
                          class="form-control"
                          v-model="currentSolicitud.Comentario"
                        />
                      </div>
                    </div>
                  </div>
  
  
                  
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Estatus</label>
                      <select
                        class="form-control"
                        v-model="currentSolicitud.Estatus"
                      >
                        <option value="Aprobado">Aprobado</option>
                        <option value="En Proceso">En Proceso</option>
                        <option value="Reprogramado">Reprogramado</option>
                        <option value="Pausado">Pausado</option>
                        <option value="Cancelado">Cancelado</option>
                      </select>
                    </div>
                  </div>
  
  
  
                  <div class="row">
                    <div class="col">
                      <div class="form-group">
                        <label class="form-label float-left ml-2">Tipo</label>
                        <select
                          class="form-control"
                          v-model="currentSolicitud.Tipo"
                        >
                          <option value="Servicio Interno">Servicio Interno</option>
                          <option value="Traslados">Traslados</option>
                          <option value="Subrogado">Subrogado</option>
                          <option value="Administrativo">Administrativo</option>
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
                        type="datetime-local"
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
                <h2 class="alert alert-success">Crea Solicitud</h2>
                <form @submit.prevent="saveSolicitud()">
                  <div class="row">
                    <div class="col">
                      <div class="form-group">
                        <label class="form-label float-left ml-2">Personal Médico</label>
                        <select
                        class="form-control"
                        v-model="solicitud.Personal_Medico_ID"
                        required
                      >
                        <option v-for="persona in personas" :key="persona.id" :value="persona.id">
                          {{ persona.Titulo_Cortesia }} {{ persona.Nombre }} {{ persona.Primer_Apellido }} {{ persona.Segundo_Apellido }}
                        </option>
                      </select>
                      </div>
                    </div>
                  </div>
  
                  <div class="row">
                    <div class="col">
                      <div class="form-group">
                        <label class="form-label float-left ml-2"
                          >Solicitud</label
                        >
                        <select
                          class="form-control"
                          v-model="solicitud.Solicitud_id"
                          required
                        >
                        <option v-for="rol in roles" :key="rol.ID" :value="rol.ID">
                          {{ rol.ID}} {{ rol.Prioridad }}
                        </option>
                        </select>
                        
                      </div>
                    </div>
                  </div>
  
                  <div class="row">
                    <div class="col">
                      <div class="form-group">
                        <label class="form-label float-left ml-2"
                          >Comentario</label
                        >
                        <input
                          type="text"
                          class="form-control"
                          v-model="solicitud.Comentario"
                          required
                        />
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
                          <option value="Aprobado">Aprobado</option>
                          <option value="En Proceso">En Proceso</option>
                          <option value="Pausado">Pausado</option>
                          <option value="Reprogramado">Reprogramado</option>
                          <option value="Cancelado">Cancelado</option>
                        </select>
                      </div>
                    </div>
                  </div>
  
  
                  <div class="row">
                    <div class="col">
                      <div class="form-group">
                        <label class="form-label float-left ml-2">Tipo</label>
                        <select
                          class="form-control"
                          v-model="solicitud.Tipo"
                          required
                        >
                          <option value="Servicio Interno">Servicio Interno</option>
                          <option value="Traslados">Traslados</option>
                          <option value="Subrogado">Subrogado</option>
                          <option value="Administrativo">Administrativo</option>
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
              onclick="window.location.href='/AprobacionModal'"
            >
              Aprobaciones Modal
            </button>
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
  
    name: "AprobacionServiciosHospitalarios",
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
        searchInput: "",
        currentPage: 1, // Página actual
        resultsPerPage: 10, // Resultados por página
  
        config: {
          dateFormat: "Y-m-d",
          inline: true,
        },
  

  
  
  
        dataTable: [],  // Datos de la tabla
        personas: [],    // Datos del personal médico
        solicitudes: []    // Datos del personal médico
  
  
        
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
        return `N°:${soli.ID} Prioridad:${soli.Prioridad} Descripción:${soli.Descripcion} Solicitud:${soli.Estatus}`;
      }
      
      return 'Datos de la Solicitud no disponible';
    }
  
  
      
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
              servicioText = String(solicitud.Personal_Medico_ID); // Si no coincide con ninguno, mantener el valor original
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
  