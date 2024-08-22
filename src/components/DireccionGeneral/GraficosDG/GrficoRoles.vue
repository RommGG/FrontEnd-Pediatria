<template>
    <div>
      <v-chart :option="chartOptions" style="width: 100%; height: 500px;" />
    </div>
  </template>
  
  <script>
  import { defineComponent, ref, onMounted } from 'vue';
  import { use } from 'echarts/core';
  import { PieChart } from 'echarts/charts';
  import { TitleComponent, TooltipComponent, LegendComponent } from 'echarts/components';
  import { CanvasRenderer } from 'echarts/renderers';
  import VChart from 'vue-echarts';
  
  // Registrar los componentes de ECharts que vamos a utilizar
  use([TitleComponent, TooltipComponent, LegendComponent, PieChart, CanvasRenderer]);
  
  export default defineComponent({
    components: {
      'v-chart': VChart,
    },
    setup() {
      const chartOptions = ref({
        title: {
          text: 'Distribución de Usuarios por Rol',
          left: 'center',
        },
        tooltip: {
          trigger: 'item',
        },
        legend: {
          orient: 'vertical',
          left: 'left',
        },
        series: [
          {
            name: 'Usuarios',
            type: 'pie',
            radius: '50%',
            data: [],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)',
              },
            },
          },
        ],
      });
  
      onMounted(async () => {
        try {
          // Obtener los datos de los roles
          const rolesResponse = await fetch('https://back-end-hospital2-0.onrender.com/roles/');
          const roles = await rolesResponse.json();
  
          // Obtener los datos de los usuarios y sus roles
          const userRolesResponse = await fetch('https://back-end-hospital2-0.onrender.com/usuario_roles/');
          const userRoles = await userRolesResponse.json();
  
          // Contar cuántos usuarios tienen cada rol
          const roleCounts = roles.map(role => {
            const count = userRoles.filter(userRole => userRole.Rol_ID === role.id).length;
            return {
              name: role.Nombre,
              value: count,
            };
          });
  
          // Configurar los datos del gráfico
          chartOptions.value.series[0].data = roleCounts;
        } catch (error) {
          console.error('Error al obtener los datos:', error);
        }
      });
  
      return {
        chartOptions,
      };
    },
  });
  </script>
  