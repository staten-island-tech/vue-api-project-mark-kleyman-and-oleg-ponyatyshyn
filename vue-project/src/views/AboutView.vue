<template>
  <div v-if="skibidi">
    <Pie :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup>
import { ref, onBeforeMount } from 'vue';
import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js';
import { Pie } from 'vue-chartjs';

ChartJS.register(ArcElement, Tooltip, Legend);

const chartData = ref({
  labels: [],
  datasets: [
    {
      backgroundColor: ['#FFC0CB', '#FFF0F5', '#FFDAB9', '#FF2973','#FFA07A', '#FF69B4', '#FFC0CB', '#FFE4E1','#FFA07A', '#FF6347', '#FF7F50', '#FFA07A','#FFB6C1', '#FFD1DC', '#E5D9BE', '#FFA07A'],
      data: []
    }
  ]
});

const chartOptions = {
  responsive: true,
};

let crashes = ref([]);
const skibidi = ref(false);

const fetchData = async () => {
  try {
    const res = await fetch('https://data.cityofnewyork.us/resource/2rb7-7eqa.json');
    const data = await res.json();

    const boroughCount = data.reduce((acc, item) => {
        const borough = item.borough_name;
        if (!acc[borough]) {
            acc[borough] = 1;
        } else {
            acc[borough]++;
        }
        return acc;
    }, {});

    const labels = Object.keys(boroughCount);
    const counts = Object.values(boroughCount);

    chartData.value.labels = labels;
    chartData.value.datasets[0].data = counts;
    skibidi.value = true;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

onBeforeMount(() => {
  fetchData();
});

</script>
