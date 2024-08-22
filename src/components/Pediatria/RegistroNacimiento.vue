<template>
    <div div class="max-w-3xl mx-auto bg-white p-6 rounded-lg shadow-lg">
        <h2 class="text-2xl font-bold mb-6">Registro de Bebé</h2>

        <div v-if="message" class="text-green-600 mb-4">
            {{ message }}
        </div>


        <form @submit.prevent="submitForm">
            <!-- Paso 1: Datos de Contacto -->
            <div v-if="currentStep === 1">
               



                <label for="nombrePadre" class="block text-sm font-medium text-gray-700 mt-4">Nombre del Padre:</label>
                <input type="text" id="nombrePadre" v-model="nombrePadre"
                    class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">


                <label for="emailContacto" class="block text-sm font-medium text-gray-700 mt-4">Email de Contacto:</label>
                <input type="email" id="emailContacto" v-model="emailContacto"
                    class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">





                <label for="nombreMadre" class="block text-sm font-medium text-gray-700 mt-4">Nombre de la Madre:</label>
                <input type="text" id="nombreMadre" v-model="nombreMadre"
                    class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">



                <label for="telefonoContacto" class="block text-sm font-medium text-gray-700 mt-4">Teléfono de
                    Contacto:</label>
                <input type="text" id="telefonoContacto" v-model="telefonoContacto"
                    class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">



            </div>
            <!-- Paso 2: Información del Bebé -->
            <div v-if="currentStep === 2">
                <h3>Información del Bebé</h3>


                <label for="fechaNacimiento" class="block text-sm font-medium text-gray-700 mt-4">Fecha de Nacimiento:</label>
                <input type="date" id="fechaNacimiento" v-model="fechaNacimiento" class="input">


                <label for="lugarNacimiento"  class="block text-sm font-medium text-gray-700 mt-4">Lugar de Nacimiento:</label>
                <select id="lugarNacimiento" v-model="lugarNacimiento"class="input">
                    <option value="">Seleccionar</option>
                    <option value="Xicotepec">Xicotepec</option>
                    <option value="Huachinango">Huachinango</option>
                    <option value="Villa Avila Camacho">Villa Avila Camacho</option>
                    <option value="Necaxa">Necaxa</option>
                </select>


                <label for="tipoNacimiento" class="block text-sm font-medium text-gray-700 mt-4">Tipo de Nacimiento:</label>
                <select id="tipoNacimiento" v-model="tipoNacimiento" class="input">
                    <option value="Normal">Normal</option>
                    <option value="Cesarea">Cesarea</option>
                </select>







               
                        <label for="horaNacimiento" class="block text-sm font-medium text-gray-700 mt-4">Hora de Nacimiento:</label>
                        <input type="time" id="horaNacimiento" v-model="horaNacimiento" class="input">
                    
                    
                        <label for="sexo" class="block text-sm font-medium text-gray-700 mt-4">Sexo:</label>
                        <select id="sexo" v-model="sexo" required class="input">
                            <option value="">Seleccionar</option>
                            <option value="Masculino">Masculino</option>
                            <option value="Femenino">Femenino</option>
                        </select>
                    
                

            </div>

            <!-- Paso 3: Frecuencias y Medidas -->
            <div v-if="currentStep === 3">
                <h3>Frecuencias y Medidas</h3>
                
                   
                        
                            <label for="temperatura" class="block text-sm font-medium text-gray-700 mt-4">Temperatura (°C):</label>
                            <input type="number" id="temperatura" v-model="temperatura" step="0.1" class="input">
                       
                        
                            <label for="frecuenciaCardiaca"class="block text-sm font-medium text-gray-700 mt-4">Frecuencia Cardíaca:</label>
                            <input type="number" id="frecuenciaCardiaca" v-model="frecuenciaCardiaca" class="input">
                       
                       
                            <label for="presionArterialSistolica"class="block text-sm font-medium text-gray-700 mt-4">Presión Arterial Sistólica:</label>
                            <input type="number" id="presionArterialSistolica" v-model="presionArterialSistolica"
                                class="input">
                        
                    

                   
                       
                            <label for="presionArterialDiastolica"class="block text-sm font-medium text-gray-700 mt-4">Presión Arterial Diastólica:</label>
                            <input type="number" id="presionArterialDiastolica" v-model="presionArterialDiastolica"
                                class="input">
                        
                        
                            <label for="longitud"class="block text-sm font-medium text-gray-700 mt-4">Longitud (cm):</label>
                            <input type="number" id="longitud" v-model="longitud" step="0.1" class="input">
                        

                        
                            <label for="peso"class="block text-sm font-medium text-gray-700 mt-4">Peso (kg):</label>
                            <input type="number" id="peso" v-model="peso" step="0.1" class="input">
                       

                            <label for="observaciones" class="block text-sm font-medium text-gray-700">Observaciones:</label><br>
                            <textarea id="observaciones" v-model="observaciones" class="input"></textarea>




                

            </div>

            <!-- Navegación del Wizard -->
            <div class="flex justify-between mt-6">
                <button type="button" class="bg-gray-600 text-white py-2 px-4 rounded-md" @click="prevStep" v-if="currentStep > 1">Anterior</button>
                <button type="button" class= "bg-blue-600 text-white py-2 px-4 rounded-md ml-auto" @click="nextStep"
                    v-if="currentStep < totalSteps">Siguiente</button>
                <button type="submit" class="bg-green-600 text-white py-2 px-4 rounded-md" v-if="currentStep === totalSteps">Registrar Bebé</button>
            </div>

        </form>


    </div>
</template>

<style>
.mensaje {

    background-color: #77cc74;
    text-align: center;
    margin-bottom: 25px;
    font-size: 20px;
    color: rgb(31 41 55 / var(--tw-bg-opacity));
    border-radius: 10px;
    padding: 5px;

}

.form-column {
    display: flex;
    flex-direction: column;
}

.grid-container {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    /* Dos columnas iguales */
    gap: 20px;
    /* Espacio entre columnas */
}

.form-group {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
    /* Espacio entre grupos de formularios */
}

.titulo {
    text-align: center;
    margin-bottom: 20px;
    font-size: 24px;
    color: rgb(31 41 55 / var(--tw-bg-opacity));
}

.form-container {
    border: 1px solid #ccc;
    padding: 20px;
    border-radius: 8px;
    background-color: #f9f9f9;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    max-width: 600px;
    margin: auto;
}

h3 {
    margin-bottom: 20px;
    color: rgb(31 41 55 / var(--tw-bg-opacity));
}

.input {
    display: block;
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
    border: 1px solid #d3cdcd;
    border-radius: 4px;
}

.boton-registrar {
    background-color: rgba(51, 54, 71, 0.884);
    /* Fondo blanco */
    color: rgb(255, 255, 255);
    /* Color del texto */
    padding: 10px 20px;
    border: 1px solid #ccc;
    /* Borde de color gris claro */
    border-radius: 4px;
    cursor: pointer;
    margin-right: 10px;
    transition: background-color 0.3s ease;
    /* Transición suave para el cambio de color */
}

.boton-registrar:hover {
    background-color: #7e7e7e;
}


.wizard-navigation {
    text-align: right;
    margin-top: 20px;
}
</style>

<script>
/* eslint-disable */
export default {
    data() {
        return {
            currentStep: 1,
            totalSteps: 3,
            sexo: '',
            fechaNacimiento: '',
            horaNacimiento: '',
            lugarNacimiento: '',
            peso: null,
            longitud: null,
            nombrePadre: '',
            nombreMadre: '',
            telefonoContacto: '',
            emailContacto: '',
            observaciones: '',
            tipoNacimiento: '',
            frecuenciaCardiaca: null,
            temperatura: null,
            presionArterialSistolica: null,
            presionArterialDiastolica: null,
            message: ''
        }
    },
    methods: {
        submitForm() {
            const TOKEN = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJOb21icmVfVXN1YXJpbyI6InlhaXIiLCJDb3JyZW9fRWxlY3Ryb25pY28iOiJzdHJpbmciLCJDb250cmFzZW5hIjoiMTIzNCIsIk51bWVyb19UZWxlZm9uaWNvX01vdmlsIjoic3RyaW5nIn0.aEXy_fgDdUHif1wzhfpxddKVg4fWAyGR3fd1p-SWDOc';

            let data = {
                sexo: this.sexo,
                fecha_nacimiento: this.fechaNacimiento,
                hora_nacimiento: this.horaNacimiento,
                lugar_nacimiento: this.lugarNacimiento,
                peso: this.peso,
                longitud: this.longitud,
                nombre_padre: this.nombrePadre,
                nombre_madre: this.nombreMadre,
                telefono_contacto: this.telefonoContacto,
                email_contacto: this.emailContacto,
                observaciones: this.observaciones,
                tipo_nacimiento: this.tipoNacimiento,
                frecuencia_cardiaca: this.frecuenciaCardiaca,
                temperatura: this.temperatura,
                presion_arterial_sistolica: this.presionArterialSistolica,
                presion_arterial_diastolica: this.presionArterialDiastolica
            };

            fetch('https://privilegecare-deploy.onrender.com/pediatria/nacimiento/', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${TOKEN}`
                },
                body: JSON.stringify(data),
            })
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Hubo un problema al registrar el bebé.');
                    }
                    return response.json();
                })
                .then(data => {
                    this.message = "¡Bebé registrado exitosamente!";
                    // Limpiar los campos del formulario después del registro exitoso
                    this.resetForm();
                })
                .catch(error => {
                    this.message = "Error: " + error.message;
                });
        },
        resetForm() {
            // Reinicia todos los campos del formulario después del registro exitoso
            this.sexo = '';
            this.fechaNacimiento = '';
            this.horaNacimiento = '';
            this.lugarNacimiento = '';
            this.peso = null;
            this.longitud = null;
            this.nombrePadre = '';
            this.nombreMadre = '';
            this.telefonoContacto = '';
            this.emailContacto = '';
            this.observaciones = '';
            this.tipoNacimiento = '';
            this.frecuenciaCardiaca = null;
            this.temperatura = null;
            this.presionArterialSistolica = null;
            this.presionArterialDiastolica = null;
        },
        nextStep() {
            if (this.isStepValid()) {
                this.currentStep++;
            }
        },
        prevStep() {
            this.currentStep--;
        },
        isStepValid() {
            if (this.currentStep === 1) {
                return this.nombrePadre && this.nombreMadre && this.emailContacto && this.telefonoContacto;
            } else if (this.currentStep === 2) {
                return this.fechaNacimiento && this.lugarNacimiento && this.tipoNacimiento && this.horaNacimiento && this.sexo;
            } else if (this.currentStep === 3) {
                return this.temperatura && this.frecuenciaCardiaca && this.presionArterialSistolica && this.presionArterialDiastolica && this.longitud && this.peso;
            }
            return true;
        },
    }
}
</script>