<template>
    <div class="container-fluid">
      <h1 style="text-align: center">
        Actividades Realizadas en el Hospital Privilege Care
      </h1>
      <br />
      <b-row>
        <b-col>
          <div class="grid grid-cols-1 md:grid-cols-4 gap-12">
            <div class="md:col-span-6">
              <h2 class="alert alert-primary text-center">
                Bitácora
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
                      <th class="py-3 px-6 text-left">Usuario</th>
                      <th class="py-3 px-6 text-left">Operación</th>
                      <th class="py-3 px-6 text-left">Tabla</th>
                      <th class="py-3 px-6 text-left">Descripción</th>
                      <th class="py-3 px-6 text-left">Estatus</th>
                      <th class="py-3 px-6 text-left">Fecha de Registro</th>

                    </tr>
                  </thead>
                  <tbody class="text-gray-600 text-xs font-light">
                    <tr v-for="(solicitud, ID) in paginatedData" :key="ID" class="text-center">
                      <td class="py-3 px-6 text-left whitespace-nowrap truncate">
                        {{ solicitud.ID }}
                      </td>

                      <td class="text-center px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                        <div class="flex flex-col  text-#334155-600">
                          <h5 class="text-sm">
                            {{ solicitud.Usuario }} 
                          </h5>
                        </div>
                      </td>

                      
                      <td>
                        <div v-if="solicitud.Operacion === 'Create'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: green">
                              <i class="fa fa-check-circle" aria-hidden="true"></i>
                            </div>
                            <span style="color: green" class="text-sm">Create</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Operacion  === 'Update'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: blue">
                              <i class="fa fa-undo" aria-hidden="true"></i>
                            </div>
                            <span style="color: Blue">Update</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Operacion  === 'Delete'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: red">
                              <i class="fa fa-times"></i>
                            </div>
                            <span style="color: red" class="text-sm">Delete'</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Operacion  === 'Cancelado'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: black">
                              <i class="fa fa-ban"></i>
                            </div>
                            <span style="color: black" class="text-sm">Cancelado</span>
                          </a>
                        </div>
                        <div v-else-if="solicitud.Operacion  === 'Read'" class="text-center">
                          <a class="iq-icons-list" href="#" target="_self">
                            <div class="icon" style="color: black">
                              <i class="fa fa-ban"></i>
                            </div>
                            <span style="color: black" class="text-sm">Read</span>
                          </a>
                        </div>
                        <div v-else class="col-12 col-md-6 col-lg-3">
                          {{ solicitud.Operacion  }}
                        </div>
                      </td>

                      <td class="text-center px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                        <div class="flex flex-col  text-green-600">
                          <h5 class="text-sm">
                            {{ solicitud.Tabla }}
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


                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
  

          </div>
        </b-col>
      </b-row>
      
      <br />

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
      <!-- <div class="container py-3">
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
        </div>
      </div> -->
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
  baseURL: 'http://127.0.0.1:8000/',
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
        api: "http://127.0.0.1:8000/bitacora/",
        solicitud: {
  
          Usuario: "",
          Operacion: "",
          Tabla: "",
          Descripcion: "",
          Estatus: "",
          Fecha_Registro: "",
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
        
      };
    },
  
    mounted() {
      body[0].classList.add("sidebar-main-menu");
      console.log("DOM is rendered");
      console.log(Object.keys(this.currentSolicitud).length);
      this.getVista();
  
  
    },
  
    unmounted() {
      body[0].classList.remove("sidebar-main-menu");
    },
  
    created() {
      console.log("DOM is created");
      this.getSolicitudes();
      this.getVista();
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
  

  
      getVista() {
        axios
          .get("http://127.0.0.1:8000/hospital/api/v1vista_estado_solicitudes/")
          .then((response) => {
            console.log(response.data);
            console.log("envio");
  
            // Reiniciar datos de la gráfica
            this.chart5.series.forEach((serie) => {
              serie.data = [];
            });
  
            // Iterar sobre los datos obtenidos y agregarlos a las series correspondientes
            response.data.forEach((item) => {
              this.chart5.series[0].data.push(item.num_aprobadas);
              this.chart5.series[1].data.push(item.num_en_proceso);
              this.chart5.series[2].data.push(item.num_no_aprobadas);
              this.chart5.series[3].data.push(item.num_canceladas);
            });
          })
          .catch((error) => {
            console.error("Error al obtener las solicitudes:", error);
          });
      },
  
      previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
      console.log("Página actual:", this.currentPage);
    },

    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
      console.log("Página actual:", this.currentPage);
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
  // Datos paginados para mostrar en la tabla
    paginatedData() {
      const startIndex = (this.currentPage - 1) * this.resultsPerPage;
      const endIndex = startIndex + this.resultsPerPage;
      return this.filteredData.slice(startIndex, endIndex);
    },

  // Total de páginas calculadas en función del número de resultados filtrados
  totalPages() {
    if (this.resultsPerPage > 0) {
      return Math.ceil(this.filteredData.length / this.resultsPerPage);
    }
    return 1; // Evitar división por cero si resultsPerPage es 0
  },

  // Datos filtrados en función del input de búsqueda
  filteredData() {
    return this.solicitudes.filter((solicitud) => {
      const matchSearchInput = Object.values(solicitud).some((value) => {
        return String(value)
          .toLowerCase()
          .includes(this.searchInput.toLowerCase());
      });
      return matchSearchInput;
    });
  }
}

  };
  </script>
  