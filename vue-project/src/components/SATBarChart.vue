<template>
    <div class="container">
      <Bar v-if="loaded" :data="chartData" />
    </div>
  </template>
  
  <script>
    import { ref, onMounted, onBeforeMount } from 'vue';
    import { Bar } from 'vue-chartjs';
    import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';
  
  ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)
  
  export default {
    name: 'BarChart',
    components: { Bar },
    setup () {
        const chartData = ref({
            labels: [],
            datasets: [{
                label: 'SAT Scores',
                data: []
            }]
    })
    async mounted () {
      this.loaded = false
  
      try {
        const response = await fetch('https://data.cityofnewyork.us/resource/f9bf-2cp4.json');
        const data = await response.json();


        this.chartdata = userlist
  
        this.loaded = true
      } catch (e) {
        console.error(e)
      }
    }
  }
  </script>