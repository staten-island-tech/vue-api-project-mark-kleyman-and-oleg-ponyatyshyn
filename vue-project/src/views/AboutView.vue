<template>
  <div class="container">
    <form class="fartpoop">
      <label id="hi" for="hi">fart</label>
      <input v-model="latitude" required/>
      <label id="hi" for="hello">poop</label>
      <input v-model="longitude" required/>
      <input id="hi" type="submit" value="submit" @click="cuzzo"> 
    </form>
    
    <div v-if="skibidi">
      <Line :data="chartData" :options="chartOptions" />
    </div>
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
      label: 'temperature °C',
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
const latitude = ref()
const longitude = ref()


const fetchData = async (insert) => {
  try {
    const res = await fetch(insert);
    const data = await res.json();
    console.log(data)
    chartData.value.labels = data.hourly.time;
    chartData.value.datasets[0].data = data.hourly.temperature_2m;
    skibidi.value = true;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};
async function getGyatt(url){
  try {
    const res = await fetch(url)
    const data = await res.json()
    console.log(data)
  } catch (error) {
    
  }
}
onBeforeMount(() => {
  fetchData('https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&hourly=temperature_2m');
  getGyatt(`http://api.openweathermap.org/geo/1.0/reverse?lat=0&lon=0&limit=5&appid=9301901bfd03743584028bab841175bd`)
});
function cuzzo(e){
  e.preventDefault();
  console.log(latitude.value, longitude.value)
  skibidi.value = false
  fetchData(`https://api.open-meteo.com/v1/forecast?latitude=${latitude.value}&longitude=${longitude.value}&hourly=temperature_2m`)
}

</script>

<style scoped>

.container{
  display: flex;
  flex-direction: column;
  
}
#hi{
  display: flex;
  margin-top: 2.5%;
  margin-bottom: 5%;
}
.fartpoop{
  display: flex;
  flex-direction: column;
  width: 10%;
  justify-content: center;
}
</style>
