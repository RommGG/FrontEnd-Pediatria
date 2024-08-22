<template>
    <div>
        <div v-if="message" class="error-message">
            {{ message }}
        </div>

        <h1 class="text-2xl font-bold mb-6">Listado de Vacunas</h1>

        <div class="tabla1">
            <table>
                <thead>
                    <tr>
                        <th>Id del Bebé</th>
                        <th>Vacuna administrada</th>
                        <th>Dosis(ml)</th>
                        <th>Vía de Administración</th>
                        <th>Lote</th>
                        <th>Fecha de Aplicación</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(vacuna, index) in vacunas" :key="index">
                        <td>{{ vacuna.nacimientos_id }}</td>
                        <td>{{ vacuna.vacuna_administrada }}</td>
                        <td>{{ vacuna.dosis }}</td>
                        <td>{{ vacuna.via_administracion }}</td>
                        <td>{{ vacuna.numero_lote }}</td>
                        <td>{{ vacuna.fecha_administracion }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
        <br>
        <router-link :to="{ name: 'Nacimientos'}">
            <button class="nav-button">
                <i class="fas fa-syringe"></i> Home
            </button>
        </router-link>
        &nbsp;
        <router-link :to="{ name: 'VacunasForm'}">
            <button class="nav-button">
                <i class="fas fa-syringe"></i> Registrar Vacuna
            </button>
        </router-link>

        <div class="pagination">
            <button @click="paginar('anterior')" :disabled="paginaActual === 1" class="pagination-button">
                Anterior
            </button>&nbsp;
            <button @click="paginar('siguiente')" :disabled="vacunas.length < itemsPorPagina" class="pagination-button">
                Siguiente
            </button>
        </div>

        <div class="pagination-info">
            Página {{ paginaActual }}
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            vacunas: [],
            message: '',
            paginaActual: 1,
            itemsPorPagina: 10,
            textoBusqueda: '' // Elimina esta línea si ya no es necesario
        };
    },
    mounted() {
        this.obtenerVacunas();
    },
    methods: {
        obtenerVacunas() {
            const id = this.$route.params.id;
            const token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJOb21icmVfVXN1YXJpbyI6InlhaXIiLCJDb3JyZW9fRWxlY3Ryb25pY28iOiJzdHJpbmciLCJDb250cmFzZW5hIjoiMTIzNCIsIk51bWVyb19UZWxlZm9uaWNvX01vdmlsIjoic3RyaW5nIn0.aEXy_fgDdUHif1wzhfpxddKVg4fWAyGR3fd1p-SWDOc'; 

            fetch(`https://privilegecare-deploy.onrender.com/pediatria/vacunas/${id}/?page=${this.paginaActual - 1}&limit=${this.itemsPorPagina}`,{
                method: 'GET',
                headers: {
                'Authorization': `Bearer ${token}`,
                'Content-Type': 'application/json'
                }
            })
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Hubo un problema al obtener la lista de Vacunas. O el paciente no tiene ninguna Vacuna');
                    }
                    return response.json();
                })
                .then(data => {
                    this.vacunas = data;
                })
                .catch(error => {
                    this.message = "Error: " + error.message;
                });
        },
        paginar(direccion) {
            if (direccion === 'anterior' && this.paginaActual > 1) {
                this.paginaActual--;
            } else if (direccion === 'siguiente') {
                this.paginaActual++;
            }
            this.obtenerVacunas(); // Corrige el nombre del método
        }
    }
}
</script>

<style scoped>
.error-message {
    color: red;
    font-weight: bold;
    margin-top: 10px;
}

h1 {
    color: rgb(31, 41, 55);
}

.tabla1 {
    margin-top: 20px;
}

table {
    width: 100%;
    border-collapse: collapse;
}

th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
}

th {
    background-color: rgb(31, 41, 55);
    color: white;
}

.nav-button {
    background-color: rgb(31, 41, 55);
    color: white;
    border: none;
    padding: 10px 20px;
    cursor: pointer;
    font-size: 16px;
    border-radius: 5px;
}

.nav-button:hover {
    background-color: #1d2d44;
}

.pagination {
    margin-top: 10px;
}

.pagination-button {
    background-color: black;
    color: white;
    border: none;
    padding: 10px 20px;
    cursor: pointer;
    font-size: 16px;
    border-radius: 5px;
}

.pagination-button:disabled {
    background-color: grey;
    cursor: not-allowed;
}

.pagination-info {
    margin-top: 10px;
    font-weight: bold;
}
</style>
