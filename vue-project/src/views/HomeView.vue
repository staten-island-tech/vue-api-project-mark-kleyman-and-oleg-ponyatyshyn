<template>
  <Bar
    id="my-chart-id"
    :options="state.chartOptions"
    :data="state.chartData"
  />
</template>

<script setup>
import { ref, reactive, onBeforeMount } from 'vue';
import { Bar } from 'vue-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);


async function getCrashes() {
  try {
    const res = await fetch('https://data.cityofnewyork.us/resource/h9gi-nx95.json');
    const data = await res.json();
    console.log(data)
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

    const counts = Object.values(crashCounts);
    chartDataSelf.push(counts[0])
    console.log(chartDataSelf)
    function onlyUnique(value, index, array) {
      return array.indexOf(value) === index;
    }
    
    const unique = crashes.value.filter(onlyUnique);
    
    chartLabels.push(unique[0])


  } catch (error) {
    console.error(error); 
  }
}
const crashes = ref([]);
let chartLabels = []
let chartDataSelf = []
const state = reactive({
  chartData: {
    labels: [chartLabels, 'skibidi', 'toilet'],
    datasets: [{ data: [chartDataSelf] }],
  },
  chartOptions: {
    responsive: true,

  },
});
onBeforeMount(() => {
  getCrashes();
  
});

</script>
