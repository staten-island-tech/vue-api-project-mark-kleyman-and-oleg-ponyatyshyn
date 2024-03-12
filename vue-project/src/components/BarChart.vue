<template>
  <div class="container">
    <Bar v-if="loaded" :data="chartData" />
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'BarChart',
  components: { Bar },
  data: () => ({
    loaded: false,
    chartData: null
  }),

  async mounted () {
    this.loaded = false

    let crashes = []

    try {
    const res = await fetch ("https://data.cityofnewyork.us/resource/h9gi-nx95.json")
    const data = await res.json();
    data.forEach((item)=>{
    crashes.push(item.contributing_factor_vehicle_1)
    crashes.sort()
    this.chartData = crashes.contributing_factor_vehicle_1;
  })
  crashes.forEach((item)=>{
    console.log(item)
  })
}
    catch (e) {
      console.error(e)
  }
}

</script>