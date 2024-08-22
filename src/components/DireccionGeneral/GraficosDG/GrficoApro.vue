<template>
    <div style="height: 400px; width: 600px;">
        <h1 class="text-1xl font-bold text-center text-gray-800 my-6">
            Estados de Solicitudes a Aprobaciones
          </h1>
        <v-chart :option="chartOptions" style="height: 400px; width: 600px;"></v-chart>
    </div>

  </template>
  
  <script>
  import { defineComponent, ref, onMounted } from 'vue';
  import { use } from 'echarts/core';
  import { CanvasRenderer } from 'echarts/renderers';
  import { BarChart } from 'echarts/charts';
  import { GridComponent, TooltipComponent, LegendComponent } from 'echarts/components';
  import VChart from 'vue-echarts';
  import axios from 'axios';
  
  use([CanvasRenderer, BarChart, GridComponent, TooltipComponent, LegendComponent]);
  
  export default defineComponent({
    name: 'AprobacionesChart',
    components: { VChart },
    setup() {
      const chartOptions = ref(null);
  
      const fetchData = async () => {
        try {
          const response = await axios.get('https://back-end-hospital2-0.onrender.com/tbb_aprobaciones/');
          const data = response.data;
  
          // Contadores por estatus y tipo
          const estatusTipoCount = {
            'Aprobado': { 'Administrativo': 0, 'Traslados': 0, 'Servicio Interno': 0, 'Subrogado': 0 },
            'En Proceso': { 'Administrativo': 0, 'Traslados': 0, 'Servicio Interno': 0, 'Subrogado': 0 },
            'Reprogramado': { 'Administrativo': 0, 'Traslados': 0, 'Servicio Interno': 0, 'Subrogado': 0 },
            'Pausado': { 'Administrativo': 0, 'Traslados': 0, 'Servicio Interno': 0, 'Subrogado': 0 },
            'Cancelado': { 'Administrativo': 0, 'Traslados': 0, 'Servicio Interno': 0, 'Subrogado': 0 }
          };
  
          // Contar solicitudes
          data.forEach(item => {
            if (estatusTipoCount.hasOwnProperty(item.Estatus) && estatusTipoCount[item.Estatus].hasOwnProperty(item.Tipo)) {
              estatusTipoCount[item.Estatus][item.Tipo]++;
            }
          });
  
          // Convertir datos para gráfico
          const xAxisData = Object.keys(estatusTipoCount);
          const types = Object.keys(estatusTipoCount['Aprobado']);
          
          // Configuración de datos de serie para el gráfico
          const seriesData = types.map(tipo => ({
            name: tipo,
            type: 'bar',
            stack: 'stack',
            data: xAxisData.map(estatus => estatusTipoCount[estatus][tipo])
          }));
  
          // Configuración del gráfico
          chartOptions.value = {
            tooltip: {
              trigger: 'axis',
              axisPointer: {
                type: 'shadow'
              }
            },
            legend: {
              data: types
            },
            grid: {
              left: '3%',
              right: '4%',
              bottom: '3%',
              containLabel: true
            },
            xAxis: {
              type: 'category',
              data: xAxisData,
              axisLabel: {
                fontSize: 8, // Ajusta el tamaño de la fuente si es necesario
                lineHeight: 50,
                align: 'center',
                verticalAlign: 'middle',
              }
            },
            yAxis: {
              type: 'value'
            },
            series: [
              ...seriesData,
              {
                name: 'Total',
                type: 'bar',
                data: xAxisData.map(estatus => 
                  Object.values(estatusTipoCount[estatus]).reduce((a, b) => a + b, 0)
                ),
                label: {
                  show: true,
                  position: 'top',
                  formatter: '{c}'
                },
                itemStyle: {
                  color: '#a0a0a0'
                }
              }
            ]
          };
        } catch (error) {
          console.error('Error fetching data:', error);
        }
      };
  
      onMounted(() => {
        fetchData();
      });
  
      return {
        chartOptions,
      };
    },
  });
  </script>
  