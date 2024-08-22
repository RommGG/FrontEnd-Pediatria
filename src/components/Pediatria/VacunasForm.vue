<template>
    <div class="max-w-3xl mx-auto bg-white p-6 rounded-lg shadow-lg">
        <h2 class="text-2xl font-bold mb-6">Registro de Vacunas</h2>

        <div v-if="message" class="text-green-600 mb-4">
            {{ message }}
        </div>
        
        <form @submit.prevent="submitForm">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="form-group">
                    <label for="nacimientos_id" class="block text-sm font-medium text-gray-700">Id del bebe</label>
                    <input type="number" id="nacimientos_id" v-model="nacimientos_id" class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                </div>

                <div class="form-group">
                    <label for="vacuna_administrada" class="block text-sm font-medium text-gray-700">Vacuna:</label>
                    <select id="vacuna_administrada" v-model="vacuna_administrada" class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                        <option value="Hepatitis">Hepatitis B (HBV)</option>
                        <option value="Difteria, Tétanos y Tos Ferina">Difteria, Tétanos y Tos Ferina (DTPa)</option>
                        <option value="Haemophilus influenzae tipo b">Haemophilus influenzae tipo b (Hib)</option>
                        <option value="Poliomielitis">Poliomielitis (VIP)</option>
                        <option value="Neumococo">Neumococo (PnC)</option>
                        <option value="Rotavirus">Rotavirus (RV)</option>
                        <option value="Sarampión, Paperas y Rubéola">Sarampión, Paperas y Rubéola (SPR)</option>
                    </select>
                </div>

                <div class="form-group">
                    <label for="dosis" class="block text-sm font-medium text-gray-700">Dosis (ml):</label>
                    <input type="number" id="dosis" v-model="dosis" step="01.0" class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                </div>

                <div class="form-group">
                    <label for="via_administracion" class="block text-sm font-medium text-gray-700">Vía de Administración</label>
                    <select id="via_administracion" v-model="via_administracion" required class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                        <option value="Intramuscular">Vía Intramuscular (IM)</option>
                        <option value="Vía Oral">Vía Oral (VO)</option>
                        <option value="Vía Subcutánea">Vía Subcutánea (SC)</option>
                    </select>
                </div>

                <div class="form-group">
                    <label for="numero_lote" class="block text-sm font-medium text-gray-700">Lote:</label>
                    <select id="numero_lote" v-model="numero_lote" class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm" required>
                        <option value="1">0001</option>
                        <option value="2">0002</option>
                        <option value="3">0003</option>
                    </select>
                </div>

                <div class="form-group ">
                    <label for="fecha_administracion" class="block text-sm font-medium text-gray-700">Fecha de Aplicación:</label>
                    <input type="date" id="fecha_administracion" v-model="fecha_administracion" class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                </div>
            </div>
            
            <div class="mt-6 flex justify-end space-x-4">
                <button type="submit" class="bg-green-600 text-white py-2 px-4 rounded-md">Registrar</button>
                <button @click="goBack" class="bg-blue-600 text-white py-2 px-4 rounded-md">Regresar</button>
            </div>
        </form>
    </div>
</template>


<style scoped>
.input {
    display: block;
    width: 100%;
    padding: 12px;
    margin-bottom: 10px;
    border: 1px solid #d3cdcd;
    border-radius: 4px;
    
}

.grid-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1rem;
}

.form-group {
    margin-bottom: 1rem;
}

.form-column {
    display: flex;
    flex-direction: column;
}

.button {
    background-color: #4CAF50;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.button:hover {
    background-color: #45a049;
}
</style>



<script>
/* eslint-disable */
export default {
    data() {
        return {
            nacimientos_id: '',
            vacuna_administrada: '',
            dosis: '',
            via_administracion: '',
            numero_lote: '',
            fecha_administracion: null,
            message: ''
        }
    },
    mounted() {
        // Extraer el último número de la URL
        const url = window.location.href;
        const match = url.match(/\/(\d+)$/);
        if (match) {
            this.nacimientos_id = match[1];
        }
    },
    methods: {
        goBack() {
            window.history.back();
        },
        submitForm() {
            const TOKEN = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJOb21icmVfVXN1YXJpbyI6InlhaXIiLCJDb3JyZW9fRWxlY3Ryb25pY28iOiJzdHJpbmciLCJDb250cmFzZW5hIjoiMTIzNCIsIk51bWVyb19UZWxlZm9uaWNvX01vdmlsIjoic3RyaW5nIn0.aEXy_fgDdUHif1wzhfpxddKVg4fWAyGR3fd1p-SWDOc'; 

            let data = {
                nacimientos_id: this.nacimientos_id,
                vacuna_administrada: this.vacuna_administrada,
                dosis: this.dosis,
                via_administracion: this.via_administracion,
                numero_lote: this.numero_lote,
                fecha_administracion: this.fecha_administracion
            };

            fetch('https://privilegecare-deploy.onrender.com/pediatria/vacunas/', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${TOKEN}`
                },
                body: JSON.stringify(data),
            })
                .then(response => {
                    if (!response.ok) {
                        throw new Error('El Paciente ya cuenta con la dosis');
                    }
                    return response.json();
                })
                .then(data => {
                    this.message = "¡Vacuna registrada exitosamente!";
                    // Limpiar los campos del formulario después del registro exitoso
                    this.resetForm();
                })
                .catch(error => {
                    this.message = "Error: " + error.message;
                });
        },
        resetForm() {
            // Reinicia todos los campos del formulario después del registro exitoso
            this.nacimientos_id = '';
            this.vacuna_administrada = '';
            this.dosis = '';
            this.via_administracion = '';
            this.numero_lote = '';
            this.fecha_administracion = null;
        }
    }
}
</script>




<!-- <template>
    <div>
        <h2>Registro de Vacunas</h2>

        <div v-if="message" style="margin-top: 10px;">
            {{ message }}
        </div>
        <form @submit.prevent="submitForm">
            <label for="nacimientos_id">Id del bebe</label><br>
            <input type="number" id="nacimientos_id" v-model="nacimientos_id" step="01.0"><br>

            <label for="vacuna_administrada">Vacuna :</label><br>
            <select id="vacuna_administrada" v-model="vacuna_administrada">
                <option value="HBV">Hepatitis B (HBV)</option>
                <option value="DTPa"> Difteria, Tétanos y Tos Ferina (DTPa)</option>
                <option value="Hib"> Haemophilus influenzae tipo b (Hib)</option>
                <option value="VIP"> Poliomielitis (VIP)</option>
                <option value="PnC"> Neumococo (PnC)</option>
                <option value="RV"> Rotavirus (RV)</option>
                <option value="SPR">Sarampión, Paperas y Rubéola (SPR)</option>
            </select><br>

            <label for="dosis">dosis (ml):</label><br>
            <input type="number" id="dosis" v-model="dosis" step="01.0"><br>

            <label for="via_administracion">Via de Administracion</label><br>
            <select id="via_administracion" v-model="via_administracion" required>
                <option value="IM">Vía Intramuscular (IM)</option>
                <option value="VO">Vía Oral (VO)</option>
                <option value="SC">Vía Subcutánea (SC)</option>
            </select><br>

            <label for="numero_lote">Lote:</label><br>
            <select id="numero_lote" v-model="numero_lote" required>
                <option value="1">0001</option>
                <option value="2">0002</option>
                <option value="3">0003</option>
            </select><br>

            <label for="fecha_administracion">Fecha de Aplicacion:</label><br>
            <input type="date" id="fecha_administracion" v-model="fecha_administracion"><br><br>
            <button type="submit">Registrar</button>
        </form>

    </div>
</template>
<script>
/* eslint-disable */
export default {
    data() {
        return {
            nacimientos_id: '',
            vacuna_administrada: '',
            dosis: '',
            via_administracion: '',
            numero_lote: '',
            fecha_administracion: null,
            message: ''
        }
    },
    methods: {
        submitForm() {
            let data = {
                nacimientos_id: this.nacimientos_id,
                vacuna_administrada: this.vacuna_administrada,
                dosis: this.dosis,
                via_administracion: this.via_administracion,
                numero_lote: this.numero_lote,
                fecha_administracion: this.fecha_administracion
            };

            fetch('https://privilegecare-deploy.onrender.com/pediatria/vacunas/', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(data),
            })
                .then(response => {
                    if (!response.ok) {
                        throw new Error('El Paciente ya cuenta con la dosis');
                    }
                    return response.json();
                })
                .then(data => {
                    this.message = "¡Vacuna registrada exitosamente!";
                    // Limpiar los campos del formulario después del registro exitoso
                    this.resetForm();
                })
                .catch(error => {
                    this.message = "Error: " + error.message;
                });
        },
        resetForm() {
            // Reinicia todos los campos del formulario después del registro exitoso
            this.nacimientos_id = '';
            this.vacuna_administrada = '';
            this.dosis = '';
            this.via_administracion = '';
            this.numero_lote = '';
            this.fecha_administracion = null;
        }
    }
}
</script> -->
