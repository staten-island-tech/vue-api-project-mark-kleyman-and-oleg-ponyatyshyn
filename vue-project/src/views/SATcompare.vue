<template>
  <div>
    <div v-if="selectedSchools.length > 0">
      <h2>School 1: {{ selectedSchools[0].school_name }}</h2>
      <!-- <p>Average SAT Score: {{ getCombinedSatScore(selectedSchools[0]) }}</p> -->

      <h2>School 2: {{ selectedSchools[1].school_name }}</h2>
      <!-- <p>Average SAT Score: {{ getCombinedSatScore(selectedSchools[1]) }}</p> -->

      <h2> Score: {{ score.value }}</h2>
      <div id="chart" v-if="gameCont">
        <Bar :data="chartData" :options="chartOptions"/>
      </div>
        <button id="fart" v-if="!gameCont" @click="guess1">School 1</button>
        <button id="fart" v-if="!gameCont"  @click="guess2">School 2</button>
        <button id="fart" v-if="gameCont" @click="continueGame">Next</button>
      
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, watchEffect, reactive } from 'vue';
import { Bar } from 'vue-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

export default {
  name: 'SchoolsDisplay',
  components: { Bar },
  setup() {
    const selectedSchools = ref([]);
    const combinedSATScores = ref([])

    const chartData = ref({
      labels: [],
      datasets: [
        {
          label: 'Average SAT Scores',
          backgroundColor: '#ff2e77',
          data: []
        }
      ]
    });
    const chartOptions = ref({
      responsive: true,
      maintainAspectRatio: false,
      scales: {
        y: {
          beginAtZero: true,
          suggestedMax: 2400
        }
      }
    });
    const fetchData = async () => {
      try {
        const response = await fetch('https://data.cityofnewyork.us/resource/f9bf-2cp4.json');
        const data = await response.json();

        const shuffledData = shuffleArray(data);
        const selected = shuffledData.slice(0, 2);

        selectedSchools.value = selected.map(school => ({
          ...school,
          combined_sat_score: calculateCombinedSatScore(school)
        }));

        combinedSATScores.value = selectedSchools.value.map(school => school.combined_sat_score);

        updateChartData();

      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };

    const shuffleArray = (array) => {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    };

    const calculateCombinedSatScore = (school) => {
      const criticalReading = parseInt(school.sat_critical_reading_avg_score);
      const math = parseInt(school.sat_math_avg_score);
      const writing = parseInt(school.sat_writing_avg_score);

      if (isNaN(criticalReading) || isNaN(math) || isNaN(writing) || criticalReading === 's' || math === 's' || writing === 's') {
        return 0;
      }

      return criticalReading + math + writing;
     };

     const updateChartData = () => {
      const newData = {
        labels: ['Option 1', 'Option 2'],
        datasets: [{
          label: 'Combined SAT Scores',
          backgroundColor: '#ff2e77',
          data: combinedSATScores.value
        }]
      };
      chartData.value = newData;
    };
    onMounted(() => {
      fetchData();
    });

    const getCombinedSatScore = (school) => {
      return school.combined_sat_score;
    };

    const score = ref(0)

    const guess1 = () => {
      if (getCombinedSatScore(selectedSchools[0] > getCombinedSatScore(selectedSchools[1]))){

      score.value++
      } else {
      score.value === 0
      }
    }

    const guess2 = () => {
      if (getCombinedSatScore(selectedSchools[1] > getCombinedSatScore(selectedSchools[0]))){

      score.value++
    } else {
    score.value === 0
    }
  }
    watchEffect(() => {
      updateChartData();
    });
    let gameCont = ref(false)
    function continueGame(){
      fetchData();
      gameCont.value = false
    }
    return {
      selectedSchools,
      combinedSATScores,
      gameCont,
      chartData,
      chartOptions,
      getCombinedSatScore,
      guess2,
      score,
      guess1,
      continueGame
    };
  },
};
</script>

<style scoped>
  #chart{
    height: 700px;
    width: 300px;
    position: 'relative'
  }
</style>
