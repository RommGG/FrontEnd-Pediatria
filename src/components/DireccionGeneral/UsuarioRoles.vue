<template>
  <div class="container-fluid">
    <h1 style="text-align: center">
      Asignación de Roles en el Hospital Privilege Care
    </h1>
    <br />
    <b-row>
      <b-col>
        <div class="grid grid-cols-1 md:grid-cols-4 gap-12">
          <div class="md:col-span-3">
            <h2 class="alert alert-primary text-center">
              Lista de Usuarios y Roles
            </h2>
            <!-- buscador de la tabla -->
            <nav class="flex items-center justify-between bg-white p-4 shadow-md">
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
                  <tr class="bg-gray-200 text-gray-600 uppercase text-xs leading-normal" >
                    <th class="py-3 px-6 text-left">No°</th>
                    <th class="py-3 px-6 text-left">Usuario</th>
                    <th class="py-3 px-6 text-left">Rol Asignado</th>
                    <th class="py-3 px-6 text-left">Estatus</th>
                    <th class="py-3 px-6 text-left">Fecha de Registro</th>
                    <th class="py-3 px-6 text-left">Fecha de Actualización</th>
                    <th class="py-3 px-6 text-left">Acciones</th>
                  </tr>
                </thead>
                <tbody class="text-gray-600 text-xs font-light">
                  <tr v-for="(solicitud, id) in paginatedData" :key="id" class="text-center" >
                    <td class="py-3 px-6 text-left whitespace-nowrap truncate">
                      {{ solicitud.Usuario_ID }}
                    </td>

                    <td class="text-center px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900" :style="{ whiteSpace: 'pre-line' }" >
                      <div class="flex flex-col items-center text-green-600">
                        <h5 class="text-sm">
                          {{ getPersonaNombre(solicitud.Usuario_ID) }}
                        </h5>
                      </div>
                    </td>

                    <td class="text-center px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                      <div class="flex flex-col items-center text-green-600">
                        <h5 class="text-sm">
                          {{ getRolNombre(solicitud.Rol_ID) }}
                        </h5>
                      </div>
                    </td>

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
                      <button class="btn btn-warning btn-sm text-sm" @click.prevent="editBtn(solicitud.Usuario_ID, solicitud.Rol_ID)">
                        Editar
                      </button>
                      <button class="btn btn-danger btn-sm" @click.prevent="deleteSolicitud(solicitud.Usuario_ID, solicitud.Rol_ID)">
                        Eliminar
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

          <!-- Edición de la Solicitud -->
          <div class="col-md-11">
            <div v-if="Object.keys(currentSolicitud).length !== 0">
              <h2 class="alert alert-warning" role="alert">
                Actualiza Roles
              </h2>

              <form @submit.prevent="updateSolicitud">
                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Usuario</label>
                      <select class="form-control" v-model="currentSolicitud.Usuario_ID" required>
                        <option v-for="usuario in usuarios" :key="usuario.Persona_ID" :value="usuario.Persona_ID">
                          {{ usuario.Nombre_Usuario }}
                        </option>
                      </select>
                    </div>
                  </div>
                </div>
              
                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Rol</label>
                      <select class="form-control" v-model="currentSolicitud.Rol_ID" required>
                        <option v-for="rol in roles" :key="rol.id" :value="rol.id">
                          {{ rol.Nombre }}
                        </option>
                      </select>
                    </div>
                  </div>
                </div>
              
                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Estado</label>
                      <select class="form-control" v-model="currentSolicitud.Estatus" required>
                        <option :value="true">Activo</option>
                        <option :value="false">Inactivo</option>
                      </select>
                    </div>
                  </div>
                </div>
              
                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Fecha de Registro</label>
                      <input
                        class="form-control"
                        v-model="currentSolicitud.Fecha_Registro"
                        required
                      />
                    </div>
                  </div>
                </div>
              
                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Fecha de Actualización</label>
                      <input
                        type="datetime-local"
                        class="form-control"
                        v-model="currentSolicitud.Fecha_Actualizacion"
                      />
                    </div>
                  </div>
                </div>
                <br />
              
                <button type="submit" class="btn btn-success float-left ml-2">
                  Actualizar
                </button>
              </form>
              
            </div>

            <!-- Crear Solicitud -->
            <div v-else>
              <h2 class="alert alert-success">Crear Rol</h2>
              <form @submit.prevent="saveSolicitud()">
                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Usuario</label>
                      <select class="form-control" v-model="solicitud.Usuario_ID" required>
                        <option v-for="usuario in usuarios" :key="usuario.Persona_ID" :value="usuario.Persona_ID">
                          {{ usuario.Nombre_Usuario }}
                        </option>
                      </select>
                    </div>
                  </div>
                </div>

                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Rol</label>
                      <select class="form-control" v-model="solicitud.Rol_ID" required>
                        <option v-for="rol in roles" :key="rol.id" :value="rol.id">
                          {{ rol.Nombre }}
                        </option>
                      </select>
                    </div>
                  </div>
                </div>

                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Estado</label>
                      <select class="form-control" v-model="solicitud.Estatus" required>
                        <option :value="true">Activo</option>
                        <option :value="false">Inactivo</option>
                      </select>
                    </div>
                  </div>
                </div>

                <div class="row">
                  <div class="col">
                    <div class="form-group">
                      <label class="form-label float-left ml-2">Fecha de Registro</label>
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
                      <label class="form-label float-left ml-2">Fecha de Actualización</label>
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
    <!-- Paginación -->
    <nav aria-label="Page navigation">
      <ul class="pagination">
        <li class="page-item" :class="{ disabled: currentPage === 1 }">
          <a class="page-link" href="#" @click.prevent="previousPage">Anterior</a>
        </li>
        <li class="page-item" :class="{ disabled: currentPage === totalPages }">
          <a class="page-link" href="#" @click.prevent="nextPage">Siguiente</a>
        </li>
      </ul>
    </nav>
  </div>

      <div class="container py-3">
        <div class="row">
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
                  src="@/assets/img/imagenes-direccionGeneral/Roles.jpg"
                  alt="Estructura Organica Hospitalaria"
                  class="mx-auto d-block"
                  style="width: 300px; height: auto; border-radius: 5px; padding-bottom: 5%;"
                />
  
              </div>
              <div class="text-center">
                <button
                  class="btn btn-primary"
                  onclick="window.location.href='/Roles'"
                >
                  Roles
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
        api: "https://back-end-hospital2-0.onrender.com/usuario_roles/",
        solicitud: {
          Usuario_ID: "",
          Rol_ID: "",
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
  
        chart5: {
          series: [
            { name: "Solcitudes Aprobadas", data: [] },
            { name: "En proceso de aprobación", data: [] },
            { name: "Solicitudes Negadas", data: [] },
            { name: "Solicitudes Canceladas", data: [] },
          ],
  
          colors: ["#089bab", "#FC9F5B", "#FF0000", "#9b9b9b"],
          chart: {
            type: "bar",
            height: 350,
            stacked: true,
            toolbar: {
              show: true,
            },
            zoom: {
              enabled: true,
            },
          },
          responsive: [
            {
              breakpoint: 480,
              options: {
                legend: {
                  position: "bottom",
                  offsetX: -10,
                  offsetY: 0,
                },
              },
            },
          ],
          plotOptions: {
            bar: {
              horizontal: false,
            },
          },
          xaxis: {
            type: "text",
            categories: [
              "Enero",
              "Febrero",
              "Marzo",
              "Abril",
              "Mayo",
              "Junio",
              "Julio",
              "Agosto",
              "Septiembre",
              "Octubre",
              "Noviembre",
              "Diciembre",
            ],
          },
          legend: {
            position: "right",
            offsetY: 40,
          },
          fill: {
            opacity: 1,
          },
        },
  
  
  
        dataTable: [],  // Datos de la tabla
        personas: [],    // Datos del personal médico
        usuarios: [], // Datos de los usuarios
        dataTable1: [],  // Datos de la tabla
        roles: []    // Datos del personal médico
  
        
      };
    },
  
    mounted() {
      body[0].classList.add("sidebar-main-menu");
      console.log("DOM is rendered");
      console.log(Object.keys(this.currentSolicitud).length);
      
      // this.getVista();
  
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
      // this.getVista();
    },
  
    methods: {

    getSolicitudes() {
      apiClient.get('usuario_roles/')
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

  const valid = this.solicitud.Usuario_ID &&
                this.solicitud.Rol_ID &&
                this.solicitud.Estatus &&
                this.solicitud.Fecha_Registro;

  if (!valid) {
    console.error('Todos los campos requeridos deben estar completos.');
    return;
  }

  apiClient.post('usuario_roles/', this.solicitud)
    .then((response) => {
      console.log('Aprobación creada:', response.data);
      this.getSolicitudes();
      this.solicitud = {}; // Limpia el formulario
    })
    .catch((error) => {
      console.error('Error al crear la aprobación:', error.response ? error.response.data : error.message);
    });
},
  

editBtn(Usuario_ID, Rol_ID) {
  console.log(Usuario_ID, Rol_ID); // Verifica que ambos IDs están definidos
  this.solicitudes.map((solicitud) => {
    if (solicitud.Usuario_ID === Usuario_ID && solicitud.Rol_ID === Rol_ID) {
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
    console.error("Usuario_ID o Rol_ID no están definidos en currentSolicitud");
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

  console.log(`Actualizando solicitud con Usuario_ID: ${Usuario_ID} y Rol_ID: ${Rol_ID}`);


  apiClient.put(`usuario_roles/${Usuario_ID}/${Rol_ID}/`, updatedSolicitud)
    .then((response) => {
      console.log(response.data);
      this.getSolicitudes(); // Actualiza la lista de solicitudes
      this.currentSolicitud = {}; // Reinicia el formulario si es necesario
    })
    .catch((error) => {
      console.error('Error al actualizar la solicitud:', error.response ? error.response.data : error.message);
    });
},




  deleteSolicitud(Usuario_ID, Rol_ID) {
    const solicitud = this.solicitudes.find(
      (solicitud) => solicitud.Usuario_ID === Usuario_ID && solicitud.Rol_ID === Rol_ID
    );

    if (
      confirm(`¿Deseas eliminar la solicitud? 
                - Usuario: ${solicitud.Usuario_ID}
                - Rol: ${solicitud.Rol_ID}
                - Estatus: ${solicitud.Estatus}
                - Fecha de Solicitud: ${this.formatearFecha(solicitud.Fecha_Registro)}
                - Fecha de Aprobación: ${this.formatearFecha(solicitud.Fecha_Actualizacion)}`)
    ) {
      apiClient.delete(`usuario_roles/${Usuario_ID}/${Rol_ID}/`)
        .then((response) => {
          console.log(response.data);
          this.getSolicitudes();
        })
        .catch((error) => {
          console.log(error);
        });
    }
  },
  
      // getVista() {
      //   axios
      //     .get("http://127.0.0.1:8000/hospital/api/v1vista_estado_solicitudes/")
      //     .then((response) => {
      //       console.log(response.data);
      //       console.log("envio");
  
      //       // Reiniciar datos de la gráfica
      //       this.chart5.series.forEach((serie) => {
      //         serie.data = [];
      //       });
  
      //       // Iterar sobre los datos obtenidos y agregarlos a las series correspondientes
      //       response.data.forEach((item) => {
      //         this.chart5.series[0].data.push(item.num_aprobadas);
      //         this.chart5.series[1].data.push(item.num_en_proceso);
      //         this.chart5.series[2].data.push(item.num_no_aprobadas);
      //         this.chart5.series[3].data.push(item.num_canceladas);
      //       });
      //     })
      //     .catch((error) => {
      //       console.error("Error al obtener las solicitudes:", error);
      //     });
      // },
  
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
        const [tableDataResponse, personasResponse, usuariosResponse] = await Promise.all([
          apiClient.get('tbb_aprobaciones/'),
          apiClient.get('persons/'),
          apiClient.get('tbb_usuarios/')
        ]);
        this.dataTable = tableDataResponse.data;
        this.personas = personasResponse.data;
        this.usuarios = usuariosResponse.data;

        console.log('DataTable:', this.dataTable);
        console.log('Personas:', this.personas);
        console.log('Usuarios:', this.usuarios);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },

    
    async fetchData1() {
      try {
        const [tableDataResponse, rolesResponse] = await Promise.all([
          apiClient.get('usuario_roles/'),
          apiClient.get('roles/')
        ]);
        this.dataTable1 = tableDataResponse.data;
        this.roles = rolesResponse.data;

        console.log('DataTable:', this.dataTable1);
        console.log('Roles:', this.roles);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },



    getUsuarioNombre(usuarioId) {
      console.log("Buscando nombre para ID de usuario:", usuarioId);
      const usuario = this.usuarios.find(u => Number(u.ID) === Number(usuarioId));
      if (usuario) {
        return `${usuario.Nombre_Usuario}`;
      }
      return "Nombre no disponible";
    },


    getPersonaNombre(personalId) {
      console.log('Buscando nombre para ID:', personalId);
      const persona = this.personas.find(p => Number(p.id) === Number(personalId));
      const usuario = this.usuarios.find(u => Number(u.Persona_ID) === Number(personalId));

      if (persona) {
        const usuarioAsignado = usuario ? usuario.Nombre_Usuario : 'Usuario no Encontrado';
        return `Nombre: ${persona.Titulo_Cortesia} ${persona.Nombre} ${persona.Primer_Apellido} ${persona.Segundo_Apellido}\nUsuario: ${usuarioAsignado}`;
      }
      return 'Nombre no disponible';
    },


    getRolNombre(rolId) {
      console.log('Buscando el Rol:', rolId);
      const rol = this.roles.find(p => Number(p.id) === Number(rolId));
      if (rol) {
        return `${rol.Nombre}`;
      }
      return 'Rol no disponible';
    }
  },






  async fetchData1() {
      try {
        const [tableDataResponse, rolesResponse] = await Promise.all([
          fetch('https://back-end-hospital2-0.onrender.com/usuario_roles/'),
          fetch('https://back-end-hospital2-0.onrender.com/roles/')
        ]);
        this.dataTable1 = await tableDataResponse.json();
        this.roles = await rolesResponse.json();
  
        console.log('DataTable:', this.dataTable1);
        console.log('Roles:', this.roles);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },

    getRolNombre(rolId) {
        console.log('Buscando el Rol:', rolId);
        // Convierte ambos IDs a número para la comparación
        const rol = this.roles.find(p => Number(p.id) === Number(rolId));
        if (rol) {
        return `${rol.Nombre}`;
        }
        return 'Rol no disponible';
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
          switch (solicitud.Nombre_Usuario) {
            case 1:
              servicioText = "Urgencias";
              break;
  
            default:
              servicioText = String(solicitud.Nombre_Usuario); // Si no coincide con ninguno, mantener el valor original
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