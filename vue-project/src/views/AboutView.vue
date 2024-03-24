<template>
  <div v-if="skibidi">
    <Line :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup>
import { ref, onBeforeMount } from 'vue';
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
} from 'chart.js'
import { Line } from 'vue-chartjs'


ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
)


const chartData = ref({
  labels: [],
  datasets: [
    {
      label: 'temperature (C)',
      backgroundColor: ['#FFC0CB', '#FFF0F5', '#FFDAB9', '#FF2973','#FFA07A', '#FF69B4', '#FFC0CB', '#FFE4E1','#FFA07A', '#FF6347', '#FF7F50', '#FFA07A','#FFB6C1', '#FFD1DC', '#E5D9BE', '#FFA07A'],
      data: []
    }
  ]
});

const chartOptions = {
  responsive: true,
  width: '500px'
};

let crashes = ref([]);
const skibidi = ref(false);

const fetchData = async () => {
  try {
    const res = await fetch('https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&hourly=temperature_2m');
    const data = await res.json();

    chartData.value.labels = data.hourly.time;
    chartData.value.datasets[0].data = data.hourly.temperature_2m;
    skibidi.value = true;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

onBeforeMount(() => {
  fetchData();
});

</script>
