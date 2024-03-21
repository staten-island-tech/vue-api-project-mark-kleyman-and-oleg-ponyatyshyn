<template>
  <Bar :data="chartData" />
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)
async function getCrashes() {
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
    const counts = Object.values(crashCounts);
    console.log(state.chartData.datasets[0].data)
      chartDataSelf.push(counts[0], counts[1])
    console.log(chartDataSelf)
    function onlyUnique(value, index, array) {
      return array.indexOf(value) === index;
    } 
    
    const unique = crashes.value.filter(onlyUnique);
    state.chartData.labels.push(unique[0], unique[1])
  } catch (error) {
    throw new Error(error)
  }
}

export default {
  name: 'BarChart',
  components: { Bar },
  data() {
    return {
      chartData: {
        labels: [ 'January', 'February', 'March'],
        datasets: [
          {
            label: 'Data One',
            backgroundColor: '#ff2e77',
            data: [40, 20, 12]
          }
        ]
      }
    }
  }
}
</script>
