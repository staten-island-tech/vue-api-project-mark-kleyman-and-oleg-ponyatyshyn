<template>
  <div v-if="skibidi">
    <Bar :data="chartData" />
  </div>
  
</template>

<script>
import { ref, onMounted, onBeforeMount } from 'vue';
import { Bar } from 'vue-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

  export default {
    name: 'BarChart',
    components: { Bar },
    setup() {
      const chartData = ref({
        labels: [],
        datasets: [
          {
            label: 'Crash Counts',
            backgroundColor: '#ff2e77',
            data: []
          }
        ]
      });
      const skibidi = ref(false)
      let crashes = ref([]);
      const fetchData = async () => {
        try {
          const res = await fetch('https://data.cityofnewyork.us/resource/h9gi-nx95.json');
          const data = await res.json();

          data.forEach((item) => {
            crashes.value.push(item.contributing_factor_vehicle_1);
          });
          const crashCounts = data.reduce((acc, item) => {
            const crash = item.contributing_factor_vehicle_1;
            if (!acc[crash]) {
              acc[crash] = 1;
            } else {
              acc[crash]++;
            }
            return acc;
          }, {});

          const labels = Object.keys(crashCounts);
          const counts = Object.values(crashCounts);

          chartData.value.labels = labels;
          chartData.value.datasets[0].data = counts;
          skibidi.value = true

        } catch (error) {
          console.error('Error fetching data:', error);
        }
      };

      onBeforeMount(() => {
        fetchData();
      });

      return { chartData, skibidi };
    }
  };
</script>
