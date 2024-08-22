<template>
	<div class="py-16">
	  <div class="flex">
		<div class="w-full bg-white dark:bg-gray-700 rounded-lg lg:rounded-l-none">
		  <h3 class="py-4 text-center text-gray-800 dark:text-white">PRIVILAGE CARE HOSPITAL</h3>
		  
		  <!-- Formulario de Personas -->
		  <Personas v-if="!showUserForm" @person-registered="handlePersonRegistered"></Personas>
  
		  <!-- Formulario de Usuario -->
		  <Usuario v-if="showUserForm" :persona-id="personaID" @user-registered="handleUserRegistered"></Usuario>
		  
		  <div v-if="!showUserForm" class="mb-12 text-center lg:w-7/12">
			<button
			  class="bg-gray-700 text-white font-bold py-2 px-4 rounded hover:bg-gray-600"
			  type="button"
			  @click="showUserForm = true"
			>
			  Continuar con Registro de Usuario
			</button>
		  </div>
		</div>
	  </div>
	</div>
  </template>
  
  <script>
  import Personas from './personas.vue';
  import Usuario from './usuario.vue';
  
  export default {
	components: {
	  Personas,
	  Usuario
	},
	data() {
	  return {
		showUserForm: false,
		personaID: null
	  };
	},
	methods: {
	  handlePersonRegistered(personaID) {
		this.personaID = personaID;
		this.showUserForm = true;
	  },
	  handleUserRegistered() {
		// Lógica después de registrar al usuario
		this.showUserForm = false;
		this.personaID = null;
		alert('Usuario registrado exitosamente.');
	  }
	}
  };
  </script>
  