<template>
  <div>
    <div v-if="message" style="margin-top: 10px;">
      {{ message }}
    </div>

    <h1 class="titulo">Listado de Bebés</h1>

    <div class="container text-center">
      <!-- Barra de búsqueda -->
      

      <div class="row align-items-center stats-container">
        <div class="col stat-box">
          <p>Total de registros en <br> la base de datos:</p>
          
          <h3 v-if="totalpSql !== null"><b>{{ totalpSql }}</b></h3>
          <p v-else>Cargando...</p>
        </div>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <br>
        <div class="col stat-box">
          <p>Total de registros <br> Femeninos:</p>
          <h3 v-if="totalFemenino !== null"><b>{{ totalFemenino }}</b></h3>
          <p v-else>Cargando...</p>
        </div>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <div class="col stat-box">
          <p>Total de registros <br>  Masculinos:</p>
          <h3 v-if="totalMasculino !== null"><b>{{ totalMasculino }}</b></h3>
          <p v-else>Cargando...</p>
        </div>
      </div>
    </div>
    <br>

    <div class="search-bar">
        <input 
          type="text" 
          v-model="textoBusqueda" 
          @input="buscarBebes"
          placeholder="Buscar por nombre del padre o madre...">
      </div>
    <div class="table-container">
      <table class="styled-table">
        <thead>
          <tr>
            <th>Id del Bebé</th>
            <th>Sexo del Bebé</th>
            <th>Nombre del Padre</th>
            <th>Nombre de la Madre</th>
            <th>Correo electrónico del contacto</th>
            <th>Fecha de nacimiento</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(bebe, index) in bebesFiltrados" :key="index">
            <td>{{ bebe.id }}</td>
            <td>{{ bebe.sexo }}</td>
            <td>{{ bebe.nombre_padre }}</td>
            <td>{{ bebe.nombre_madre }}</td>
            <td>{{ bebe.email_contacto }}</td>
            <td>{{ bebe.fecha_nacimiento }}</td>
            <td>
              <button @click="eliminarBebe(bebe.id)" class="action-btn delete-btn">
                <i class="fas fa-trash-alt"></i> Eliminar
              </button>
              <br>
              &nbsp;&nbsp;&nbsp;&nbsp;

              <router-link :to="{ name: 'editar', params: { id: bebe.id } }" class="action-btn">
                <button>
                  <i class="fas fa-edit"></i> Editar Bebé
                </button>
              </router-link>
              <br>
              &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
              <router-link :to="{ name: 'TablaVacunas',params: { id: bebe.id } }" class="action-btn">
                <button>
                  <i class="fas fa-syringe"></i> Ver Vacunas
                </button>
              </router-link>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="pagination-controls">
      <button @click="paginar('anterior')" :disabled="paginaActual === 1" class="pagination-btn">
        Anterior
      </button>
      &nbsp;
      <button @click="paginar('siguiente')" :disabled="bebes.length < itemsPorPagina" class="pagination-btn">
        Siguiente
      </button>
    </div>

    <div style="margin-top: 10px;">
      Página {{ paginaActual }}
    </div>
  </div>
</template>

<style scoped>
/* Estilos generales */

.titulo {
    text-align: center;
    margin-bottom: 20px;
    font-size: 24px;
    color: rgb(31 41 55 / var(--tw-bg-opacity));
}


.container {
  max-width: 100%;
  margin: 0 auto;
  padding: 0 20px;
}

.search-bar input {
  width: 100%;
  padding: 10px;
  border-radius: 8px;
  border: 1px solid #ccc;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.1);
}

.stats-container {
  
  justify-content: space-around;
  margin-bottom: 20px;
}

.stat-box {
  background-color: #5f7892;
  padding: 8px; /* Espacio interno reducido */
  margin-bottom: 10px; /* Espacio entre cada contenedor */
  border-radius: 8px;
  box-shadow: 0px 0px 5px rgba(187, 29, 29, 0.1);
  text-align: center; /* Centra el texto y los elementos dentro del contenedor */
  font-size: 14px; /* Tamaño de fuente más pequeño */
}

.stat-img {
  width: 50px;
  margin: 10px 0;
  display: block; /* Hace que la imagen sea un bloque */

}

.table-container {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  overflow-x: auto;
}

.styled-table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
  background-color: #58728bde;
}

.styled-table thead tr {
  background-color: #343a40;
  color: #666d86;
}

.styled-table th, .styled-table td {
  padding: 12px;
  border: 1px solid #1c204e;
}

.styled-table tr:nth-child(even) {
  background-color: #f2f2f2;
}

/* Botones de acciones */
.action-btn {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 8px 12px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
  cursor: pointer;
  border-radius: 4px;
  margin-right: 5px;
}

.action-btn:hover {
  background-color: #45a049;
}



/* Controles de paginación */
.pagination-controls {
  margin-top: 20px;
  display: flex;
  justify-content: center;
}

.pagination-btn {
  background-color: #343a40;
  color: white;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
  border-radius: 4px;
}

.pagination-btn:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
}

.pagination-btn:hover:not(:disabled) {
  background-color: #495057;
}
</style>

  <script>
  /* eslint-disable */
  export default {
    data() {
      return {
        bebes: [], 
        message: '',
        paginaActual: 1,
        itemsPorPagina: 10,
        totalpSql: null,
        totalMasculino: null,
        totalFemenino: null,
        textoBusqueda: '', // Nuevo dato para la búsqueda
      };
    },
    computed: {
      // Computed property para filtrar los bebés según la búsqueda
      bebesFiltrados() {
        if (this.textoBusqueda.trim() === '') {
          return this.bebes;
        }
        const busqueda = this.textoBusqueda.toLowerCase();
        return this.bebes.filter(bebe =>
          bebe.nombre_padre.toLowerCase().includes(busqueda) ||
          bebe.nombre_madre.toLowerCase().includes(busqueda)
        );
      }
    },
    mounted() {
      this.obtenerBebes();
      this.obtenerTotalSQL();
      setInterval(this.obtenerTotalSQL, 2000);
    },
    methods: {
  
      obtenerTotalSQL() {
        const token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJOb21icmVfVXN1YXJpbyI6InlhaXIiLCJDb3JyZW9fRWxlY3Ryb25pY28iOiJzdHJpbmciLCJDb250cmFzZW5hIjoiMTIzNCIsIk51bWVyb19UZWxlZm9uaWNvX01vdmlsIjoic3RyaW5nIn0.aEXy_fgDdUHif1wzhfpxddKVg4fWAyGR3fd1p-SWDOc'; 
        fetch('https://privilegecare-deploy.onrender.com/pediatria/nacimientogenero/',{
          method: 'GET',
          headers: {
            'Authorization': `Bearer ${token}`,
            'Content-Type': 'application/json'
          }
        })
          .then(response => {
            if (!response.ok) {
              throw new Error('Error al obtener los datos');
            }
            return response.json();
          })
          .then(data => {
            if (Array.isArray(data) && data.length > 0) {
              this.totalpSql = data[0].total_pacientes;
              this.totalMasculino = data[0].total_masculino;
              this.totalFemenino = data[0].total_femenino;
            } else {
              throw new Error('No se encontraron datos válidos');
            }
          })
          .catch(error => {
            this.totalpSql = 'Error al obtener los datos';
            this.totalMasculino = 'Error';
            this.totalFemenino = 'Error';
          });
      },
  
      obtenerBebes() {
        const token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJOb21icmVfVXN1YXJpbyI6InlhaXIiLCJDb3JyZW9fRWxlY3Ryb25pY28iOiJzdHJpbmciLCJDb250cmFzZW5hIjoiMTIzNCIsIk51bWVyb19UZWxlZm9uaWNvX01vdmlsIjoic3RyaW5nIn0.aEXy_fgDdUHif1wzhfpxddKVg4fWAyGR3fd1p-SWDOc'; 
        fetch(`https://privilegecare-deploy.onrender.com/pediatria/nacimientos/?page=${this.paginaActual - 1}&limit=${this.itemsPorPagina}`,{
          method: 'GET',
          headers: {
            'Authorization': `Bearer ${token}`,
            'Content-Type': 'application/json'
          }
        })
          .then(response => {
            if (!response.ok) {
              throw new Error('Hubo un problema al obtener la lista de bebés.');
            }
            return response.json();
          })
          .then(data => {
            this.bebes = data;
          })
          .catch(error => {
            this.message = "Error: " + error.message;
          });
      },
  
      eliminarBebe(id) {
        const token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJOb21icmVfVXN1YXJpbyI6InlhaXIiLCJDb3JyZW9fRWxlY3Ryb25pY28iOiJzdHJpbmciLCJDb250cmFzZW5hIjoiMTIzNCIsIk51bWVyb19UZWxlZm9uaWNvX01vdmlsIjoic3RyaW5nIn0.aEXy_fgDdUHif1wzhfpxddKVg4fWAyGR3fd1p-SWDOc'; 
        fetch(`https://privilegecare-deploy.onrender.com/pediatria/nacimiento/${id}`, {
          method: 'DELETE',
          headers: {
            'Authorization': `Bearer ${token}`,
            'Content-Type': 'application/json'
          }
        })
          .then(response => {
            if (!response.ok) {
              throw new Error('Hubo un problema al eliminar el bebé.');
            }
            this.message = "¡Bebé eliminado exitosamente!";
            this.obtenerBebes();
            setTimeout(() => {
              this.message = '';
            }, 3000);
          })
          .catch(error => {
            this.message = "Error: " + error.message;
          });
      },
  
      buscarBebes() {
        // Este método se llama cada vez que se escribe en la barra de búsqueda.
      },
  
      paginar(direccion) {
        if (direccion === 'anterior' && this.paginaActual > 1) {
          this.paginaActual--;
        } else if (direccion === 'siguiente') {
          this.paginaActual++;
        }
        this.obtenerBebes();
      }
    }
  }
  </script>
  
