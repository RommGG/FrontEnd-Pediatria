<template>
   <button data-drawer-target="sidebar-multi-level-sidebar" data-drawer-toggle="sidebar-multi-level-sidebar"
      aria-controls="sidebar-multi-level-sidebar" type="button"
      class="inline-flex items-center p-2 mt-2 ms-3 text-sm text-gray-500 rounded-lg sm:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600">
      <span class="sr-only">Open sidebar</span>
      <svg class="w-6 h-6" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20"
         xmlns="http://www.w3.org/2000/svg">
         <path clip-rule="evenodd" fill-rule="evenodd"
            d="M2 4.75A.75.75 0 012.75 4h14.5a.75.75 0 010 1.5H2.75A.75.75 0 012 4.75zm0 10.5a.75.75 0 01.75-.75h7.5a.75.75 0 010 1.5h-7.5a.75.75 0 01-.75-.75zM2 10a.75.75 0 01.75-.75h14.5a.75.75 0 010 1.5H2.75A.75.75 0 012 10z">
         </path>
      </svg>
   </button>

   <aside id="sidebar-multi-level-sidebar"
      class="fixed top-0 left-0 z-40 w-64 h-screen transition-transform -translate-x-full sm:translate-x-0"
      aria-label="Sidebar">
      <div class="h-full px-3 py-4 overflow-y-auto bg-gray-50 dark:bg-gray-800">
         <ul class="space-y-2 font-medium">
            <li>
               <a href="#"
                  class="flex items-center p-2 text-gray-900 rounded-lg dark:text-white hover:bg-gray-100 dark:hover:bg-gray-700 group">
                  <svg
                     class="w-5 h-5 text-gray-500 transition duration-75 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-white"
                     aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 22 21">
                     <path
                        d="M16.975 11H10V4.025a1 1 0 0 0-1.066-.998 8.5 8.5 0 1 0 9.039 9.039.999.999 0 0 0-1-1.066h.002Z" />
                     <path
                        d="M12.5 0c-.157 0-.311.01-.565.027A1 1 0 0 0 11 1.02V10h8.975a1 1 0 0 0 1-.935c.013-.188.028-.374.028-.565A8.51 8.51 0 0 0 12.5 0Z" />
                  </svg>
                  <span class="ms-3">Dashboard</span>

               </a>

            </li>
            <li>

               <div class="flex items-center justify-center">
                  <!-- Mostrar la imagen del usuario si está disponible -->
                  <img v-if="userPhoto1" :src="userPhoto1" alt="Fotografía del usuario"
                     class="w-24 h-24 rounded-full object-cover border-2 border-gray-300 shadow-md" />
                  <!-- Mostrar un mensaje alternativo si no hay fotografía -->
                  <p v-else class="text-gray-500">No se encontró la fotografía del usuario.</p>
               </div>


               <div
                  class="flex items-center justify-center p-2 text-gray-900 rounded-lg dark:text-white hover:bg-gray-100 dark:hover:bg-gray-700 group">
                  <h1
                     class="flex items-center justify-center text-xs width: 1px; mt transition-transform transform hover:scale-110">
                     Bienvenid@: {{ userEmail1 }} </h1>
               </div>
               <a
                  class="flex items-center justify-center p-2 text-gray-900 rounded-lg dark:text-white hover:bg-gray-100 dark:hover:bg-gray-700 group">
                  <h1 class="text-indigo-950 font-bold text-center uppercase text-md-center flex items-center p-2">
                     {{ getRoleName() }}
                  </h1>
               </a>

            </li>

            <!-- Direccion General -->
            <li v-if="userRole === 'Administrativo' || userRole === 'Direccion General'">
               <button type="button"
                  class="flex items-center w-full p-2 text-base text-gray-900 transition duration-75 rounded-lg group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
                  aria-controls="dropdown-example8" data-collapse-toggle="dropdown-example8">
                  <span class="material-symbols-outlined">
                     folder_managed
                  </span>
                  <span class="flex-1 ms-3 text-left rtl:text-right whitespace-nowrap">Dirección General</span>
                  <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                     viewBox="0 0 10 6">
                     <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="m1 1 4 4 4-4" />
                  </svg>
               </button>
               <ul id="dropdown-example8" class="hidden py-2 space-y-2">

                  <RouterLink to="/aprobacionSM">
                     <li>
                        <a href="#"
                           class="flex items-center px-4 py-2 text-gray-900 mt transition-transform transform hover:scale-110">
                           <svg width="20px" height="20px" viewBox="0 0 1024 1024" class="icon mr-3" version="1.1"
                              xmlns="http://www.w3.org/2000/svg">
                              <path d="M601.5 531.8h278.8v16H601.5zM639.3 657.4h224v16h-224zM686.8 779h160.8v16H686.8z"
                                 fill="#F73737" />
                              <path
                                 d="M216.3 927.8H62.2V425.6h155.4l-1.3 502.2z m-110.1-44h66.2l1.1-414.2h-67.3v414.2zM822.1 927.8H268.9l-0.4-502L633.3 96.2l85.2 91.5-66.8 196.7h310L822.1 927.8z m-509.3-44H788l117-455.4H655.8l-65.5-0.1 78.1-229.9-37.8-40.5-318.1 287.4 0.3 438.5z"
                                 fill="#353535" />
                           </svg>
                           Aprobación S. M.
                        </a>
                     </li>
                  </RouterLink>

                  <RouterLink to="/Roles">
                     <li>
                        <a href="#"
                           class="flex items-center px-4 py-2 text-gray-900 transition-transform transform hover:scale-110">
                           <svg width="35px" height="35px" viewBox="0 0 1024 1024" class="icon mr-3" version="1.1"
                              xmlns="http://www.w3.org/2000/svg">
                              <path d="M512 616.2m-10 0a10 10 0 1 0 20 0 10 10 0 1 0-20 0Z" fill="#E73B37" />
                              <path d="M511.6 656.9m-10 0a10 10 0 1 0 20 0 10 10 0 1 0-20 0Z" fill="#E73B37" />
                              <path d="M512.4 697.7m-10 0a10 10 0 1 0 20 0 10 10 0 1 0-20 0Z" fill="#E73B37" />
                              <path
                                 d="M512 130.8c42.1 0 81.7 16.4 111.5 46.2s46.2 69.4 46.2 111.5-16.4 81.7-46.2 111.5c-29.8 29.8-69.4 46.2-111.5 46.2s-81.7-16.4-111.5-46.2c-29.8-29.8-46.2-69.4-46.2-111.5s16.4-81.7 46.2-111.5 69.4-46.2 111.5-46.2m0-44c-111.4 0-201.6 90.3-201.6 201.6C310.4 399.8 400.7 490 512 490c111.4 0 201.6-90.3 201.6-201.6S623.3 86.8 512 86.8zM512.3 523.5L84 681.4v255.7h856V681.4L512.3 523.5zM896 893.1H128V712.6l384.3-142.4L896 712.6v180.5z"
                                 fill="#39393A" />
                              <path
                                 d="M555.4 585.3l-1.4-0.5v159.9c0 11.7-4.8 22.3-12.4 30-7.7 7.7-18.3 12.4-30 12.4-23.4 0-42.4-19-42.4-42.4V585.3l-1.4 0.5-14.6 5.2v153.8c0 32.2 26.2 58.4 58.4 58.4S570 777 570 744.8V590.5l-14.6-5.2z"
                                 fill="#E73B37" />
                           </svg>
                           Estrucutura Organica Hospitalaria
                        </a>
                     </li>
                  </RouterLink>

                  <RouterLink to="/gradicosDG">
                     <li>
                        <a href="#"
                           class="flex items-center px-4 py-2 text-gray-900 transition-transform transform hover:scale-110">
                           <svg width="20px" height="20px" viewBox="0 0 1024 1024" class="icon mr-3" version="1.1"
                              xmlns="http://www.w3.org/2000/svg">
                              <path d="M928.1 881v44H95.9V99h44v782z" fill="#39393A" />
                              <path
                                 d="M352 435.7v403.4H204V435.7h148m22-22H182v447.4h192V413.7zM608 307.9v531.2H460V307.9h148m22-22H438v575.2h192V285.9z"
                                 fill="#39393A" />
                              <path d="M866.1 177.3v663.9H714V177.3h152.1m20-20H694v703.9h192V157.3h0.1z"
                                 fill="#E73B37" />
                           </svg>
                           Estadistica
                        </a>
                     </li>
                  </RouterLink>

                  <RouterLink to="/Bitacora">
                     <li>
                        <a href="#"
                           class="flex items-center px-4 py-2 text-gray-900 transition-transform transform hover:scale-110">
                           <svg width="20px" height="20px" viewBox="0 0 1024 1024" class="icon mr-3 " version="1.1"
                              xmlns="http://www.w3.org/2000/svg">
                              <path d="M719.8 651.8m-10 0a10 10 0 1 0 20 0 10 10 0 1 0-20 0Z" fill="#E73B37" />
                              <path
                                 d="M512.1 64H172v896h680V385.6L512.1 64z m278.8 324.3h-280v-265l280 265zM808 916H216V108h278.6l0.2 0.2v296.2h312.9l0.2 0.2V916z"
                                 fill="#39393A" />
                              <path d="M280.5 530h325.9v16H280.5z" fill="#39393A" />
                              <path d="M639.5 530h90.2v16h-90.2z" fill="#E73B37" />
                              <path d="M403.5 641.8h277v16h-277z" fill="#39393A" />
                              <path d="M280.6 641.8h91.2v16h-91.2z" fill="#E73B37" />
                              <path d="M279.9 753.7h326.5v16H279.9z" fill="#39393A" />
                              <path d="M655.8 753.7h73.9v16h-73.9z" fill="#E73B37" />
                           </svg> Bitacora
                        </a>
                     </li>
                  </RouterLink>
               </ul>
            </li>

            <!-- Recursos Humanos -->
            <li
               v-if="userRole === 'Recursos Humanos' || userRole === 'Medico Especialista' || userRole === 'Medico General' || userRole === 'Administrativo' || userRole === 'Direccion General'">
               <button type="button"
                  class="flex items-center w-full p-2 text-base text-gray-900 transition duration-75 rounded-lg group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
                  aria-controls="dropdown-example0" data-collapse-toggle="dropdown-example0">
                  <span class="material-symbols-outlined">
                     badge
                  </span>
                  <span class="flex-1 ms-3 text-left rtl:text-right whitespace-nowrap">Recursos Humanos</span>
                  <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                     viewBox="0 0 10 6">
                     <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="m1 1 4 4 4-4" />
                  </svg>
               </button>
               <ul id="dropdown-example0" class="hidden py-2 space-y-2">
                  <li>
                     <RouterLink to="/personas">
                        <a href="#"
                           class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Personas</a>
                     </RouterLink>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Puestos</a>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Personal
                        Médico</a>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Horarios</a>
                  </li>
               </ul>
            </li>

            <!-- Registros Medicos -->
            <li
               v-if="userRole === 'Registros Médicos' || userRole === 'Medico Especialista' || userRole === 'Direccion General' || userRole === 'Administrativo'">
               <button type="button"
                  class="flex items-center w-full p-2 text-base text-gray-900 transition duration-75 rounded-lg group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
                  aria-controls="dropdown-example7" data-collapse-toggle="dropdown-example7">
                  <span class="material-symbols-outlined">
                     clinical_notes
                  </span>
                  <span class="flex-1 ms-3 text-left rtl:text-right whitespace-nowrap">Registros Médicos</span>
                  <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                     viewBox="0 0 10 6">
                     <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="m1 1 4 4 4-4" />
                  </svg>
               </button>
               <ul id="dropdown-example7" class="hidden py-2 space-y-2">

                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Citas</a>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Expediente
                        Médico</a>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Receta
                        Médica</a>
                  </li>
               </ul>
            </li>

            <!-- Pediatria - -->
            <li
               v-if="userRole === 'Pediatría' || userRole === 'Medico Especialista' || userRole === 'Enfermero' || userRole === 'Administrativo' || userRole === 'Direccion General'">
               <button type="button"
                  class="flex items-center w-full p-2 text-base text-gray-900 transition duration-75 rounded-lg group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
                  aria-controls="dropdown-example4" data-collapse-toggle="dropdown-example4">
                  <span class="material-symbols-outlined">
                     pediatrics
                  </span>
                  <span class="flex-1 ms-3 text-left rtl:text-right whitespace-nowrap">Pediatría</span>
                  <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                     viewBox="0 0 10 6">
                     <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="m1 1 4 4 4-4" />
                  </svg>
               </button>
               <ul id="dropdown-example4" class="hidden py-2 space-y-2">

                  <li>
                     <RouterLink to="RegistroNacimiento">
                        <a href=""
                           class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Registrar
                           Nacimiento</a>
                     </RouterLink>
                  </li>
                  <RouterLink to="Nacimientos">
                     <li>
                        <a href=""
                           class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Nacimientos</a>
                     </li>
                  </RouterLink>

                  <!-- <RouterLink to="VacunasForm">
                     <li>
                        <a href=""
                           class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Registrar Vacunas</a>
                     </li>
                  </RouterLink> -->

                  
               </ul>
            </li>

            <!-- Farmacia - Medicamentos - -->
            <li
               v-if="userRole === 'Medico General' || userRole === 'Enfermero' || userRole === 'Medico Especialista' || userRole === 'Administrativo' || userRole === 'Farmacia'">
               <button type="button"
                  class="flex items-center w-full p-2 text-base text-gray-900 transition duration-75 rounded-lg group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
                  aria-controls="dropdown-example5" data-collapse-toggle="dropdown-example5">
                  <span class="material-symbols-outlined">
                     medication
                  </span>
                  <span class="flex-1 ms-3 text-left rtl:text-right whitespace-nowrap">Farmacia</span>
                  <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                     viewBox="0 0 10 6">
                     <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="m1 1 4 4 4-4" />
                  </svg>
               </button>
               <ul id="dropdown-example5" class="hidden py-2 space-y-2">

                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Consumilbles</a>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Lote
                        Medicamentos</a>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Medicamentos</a>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Dispensación</a>
                  </li>
               </ul>
            </li>

            <!-- Estudios - -->
            <li
               v-if="userRole === 'Radiologia' || userRole === 'Paciente' || userRole === 'Medico Especialista' || userRole === 'Administrativo' || userRole === 'Direccion General'">
               <button type="button"
                  class="flex items-center w-full p-2 text-base text-gray-900 transition duration-75 rounded-lg group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
                  aria-controls="dropdown-example3" data-collapse-toggle="dropdown-example3">
                  <span class="material-symbols-outlined">
                     lab_profile
                  </span>
                  <span class="flex-1 ms-3 text-left rtl:text-right whitespace-nowrap">Estudios</span>
                  <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                     viewBox="0 0 10 6">
                     <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="m1 1 4 4 4-4" />
                  </svg>
               </button>
               <ul id="dropdown-example3" class="hidden py-2 space-y-2">

                  <li>
                     <RouterLink to="/estudio">
                        <a href="#"
                           class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Registrar
                           Estudios</a>
                     </RouterLink>
                  </li>
                  <li>
                     <RouterLink to="/tablaEstudios">
                        <a href="#"
                           class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Listar
                           Estudios</a>
                     </RouterLink>
                  </li>
                  <li>
                     <RouterLink to="/resultadosEstudio">
                        <a href="#"
                           class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Registrar
                           Resultados de Estudios</a>
                     </RouterLink>
                  </li>
                  <li>
                     <RouterLink to="/tablaResultadoEstudio">
                        <a href="#"
                           class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Listar
                           Resultados de Estudios</a>
                     </RouterLink>
                  </li>
               </ul>
            </li>

            <!-- Transplante - -->
            <li
               v-if="userRole === 'Trasplantes' || userRole === 'Médico Especialista' || userRole === 'Médico General' || userRole === 'Administrativo' || userRole === 'Direccion General'">
               <button type="button"
                  class="flex items-center w-full p-2 text-base text-gray-900 transition duration-75 rounded-lg group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
                  aria-controls="dropdown-example6" data-collapse-toggle="dropdown-example6">
                  <span class="material-symbols-outlined">
                     pulmonology
                  </span>
                  <span class="flex-1 ms-3 text-left rtl:text-right whitespace-nowrap">Transplante</span>
                  <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                     viewBox="0 0 10 6">
                     <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="m1 1 4 4 4-4" />
                  </svg>
               </button>
               <ul id="dropdown-example6" class="hidden py-2 space-y-2">

                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Organos</a>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Detalle
                        de Organo</a>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Solicitud
                        de Transplante</a>
                  </li>
               </ul>
            </li>

            <!-- Cirugias - -->
            <li
               v-if="userRole === 'Programacion Quirurgica' || userRole === 'Médico Especialista' || userRole === 'Direccion General' || userRole === 'Administrativo'">
               <button type="button"
                  class="flex items-center w-full p-2 text-base text-gray-900 transition duration-75 rounded-lg group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700"
                  aria-controls="dropdown-example2" data-collapse-toggle="dropdown-example2">
                  <span class="material-symbols-outlined">
                     surgical
                  </span>
                  <span class="flex-1 ms-3 text-left rtl:text-right whitespace-nowrap">Cirugías</span>
                  <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                     viewBox="0 0 10 6">
                     <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="m1 1 4 4 4-4" />
                  </svg>
               </button>
               <ul id="dropdown-example2" class="hidden py-2 space-y-2">
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Cirugias</a>
                  </li>
                  <li>
                     <a href="#"
                        class="flex items-center w-full p-2 text-gray-900 transition duration-75 rounded-lg pl-11 group hover:bg-gray-100 dark:text-white dark:hover:bg-gray-700">Calendario
                        de Cirugias</a>
                  </li>
               </ul>
            </li>

            <li>
               <a href="/"
                  class="flex items-center p-2 text-gray-900 rounded-lg dark:text-white hover:bg-gray-100 dark:hover:bg-gray-700 group">
                  <svg
                     class="flex-shrink-0 w-5 h-5 text-gray-500 transition duration-75 dark:text-gray-400 group-hover:text-gray-900 dark:group-hover:text-white"
                     aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 18 16">
                     <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="M1 8h11m0 0L8 4m4 4-4 4m4-11h3a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2h-3" />
                  </svg>
                  <span class="flex-1 ms-3 whitespace-nowrap" @click="logout">Cerrar Sesión</span>
               </a>
            </li>

         </ul>
      </div>



   </aside>

   <div class="p-4 sm:ml-64">
      <router-view></router-view>
   </div>


   <!-- Se comento el pie de pagina para fines de maquetacion del sitio -->
   <!-- <PiePaginaView style="position: fixed; bottom: 0; width: 100%;" ></PiePaginaView> -->


</template>



<script>
import PiePaginaView from './pie-pagina.vue';
import axios from 'axios';

const apiClient = axios.create({
   baseURL: "https://back-end-hospital2-0.onrender.com/",
   headers: {
      Authorization: `Bearer ${localStorage.getItem("token")}`,
   },
});

export default {
   data() {
      return {
         usuario: {
            Usuario: "",
            Contrasena: "",
         },
         mensaje: "",
         token: "",
         token: localStorage.getItem('token') || '',
         userEmail1: localStorage.getItem('correo') || '',
         roles: JSON.parse(localStorage.getItem('roles')) || [],
         roleId: localStorage.getItem('role') || '',
         userPhoto1: '', // Define userPhoto aquí
      };
   },
   components: {
      PiePaginaView,

   },

   computed: {

      userRole() {
         // Busca el rol basado en el roleId
         const role = this.roles.find(role => role.id === parseInt(this.roleId));
         return role ? role.Nombre : 'Rol no encontrado';
      },

   },


   methods: {

      getRoleName() {
         // Usa el ID almacenado para buscar el nombre del rol
         const role = this.roles.find(role => role.id === parseInt(this.roleId));
         return role ? role.Nombre : 'Rol no encontrado';
      },

      fetchRoles() {
         // Obtén los roles y guárdalos en localStorage
         apiClient.get('https://back-end-hospital2-0.onrender.com/roles/')
            .then(response => {
               this.roles = response.data;
               localStorage.setItem('roles', JSON.stringify(this.roles));
            })
            .catch(error => {
               console.error('Error al obtener roles:', error);
            });
      },


      async fetchUserData() {
         try {
            // Realiza la solicitud a tbb_usuarios usando apiClient
            const response = await apiClient.get('tbb_usuarios/');
            const data = response.data;

            // Verifica que la respuesta sea un arreglo y que tenga al menos un elemento
            if (Array.isArray(data) && data.length > 0) {
               // Asume que el primer objeto en el arreglo es el que necesitas
               const usuario = data.find(user => user.Correo_Electronico === this.userEmail1);

               if (usuario) {
                  this.userEmail1 = usuario.Correo_Electronico;

                  // Verifica el valor de Persona_ID
                  if (usuario.Persona_ID) {
                     // Realiza la solicitud a /persons usando apiClient
                     const personResponse = await apiClient.get(`person/${usuario.Persona_ID}`);
                     const personData = personResponse.data;

                     // Imprimir los datos para depuración
                     console.log('Datos de la persona:', personData);

                     // Asegúrate de construir una URL absoluta para la fotografía
                     if (personData.Fotografia) {
                        // Elimina cualquier prefijo './' si existe
                        const cleanedPhotoPath = personData.Fotografia.replace('./', '');
                        this.userPhoto1 = `https://back-end-hospital2-0.onrender.com/${cleanedPhotoPath}`;
                        console.log('Fotografía actualizada:', this.userPhoto1);
                     } else {
                        this.userPhoto1 = ''; // O una URL de imagen predeterminada
                        console.error('Fotografía no disponible.');
                     }
                  } else {
                     console.error('Persona_ID no está definido.');
                     this.userPhoto1 = ''; // O una URL de imagen predeterminada
                  }
               } else {
                  console.error('Usuario no encontrado.');
                  this.userPhoto1 = ''; // O una URL de imagen predeterminada
               }
            } else {
               console.error('Datos de tbb_usuarios no están disponibles.');
               this.userPhoto1 = ''; // O una URL de imagen predeterminada
            }
         } catch (error) {
            console.error('Error fetching user data:', error);
            this.userPhoto1 = ''; // O una URL de imagen predeterminada
         }
      },


      logout() {
         // Borra el localStorage
         localStorage.removeItem('correo');
         localStorage.removeItem('role');
         localStorage.removeItem('token');
         localStorage.removeItem('roles');
         // Redirige a la página de inicio de sesión
         this.$router.push('/');
      },

   },
   mounted() {
      // Si los roles no están en localStorage, obténlos
      if (!this.roles.length) {
         this.fetchRoles();
      }

      this.fetchUserData()

   }
};
</script>