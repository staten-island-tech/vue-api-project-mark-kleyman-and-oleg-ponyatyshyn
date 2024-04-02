<template>
  <div>
    <h1 class="mainHeader">SAT Guessing Game</h1>
    <p class="directions">Directions: Guess which school had the higher SAT scores! Click on one of the buttons below to make your guess!</p>
    <div v-if="selectedSchools.length > 0">
      <h2>School 1: {{ selectedSchools[0].school_name }}</h2>

      <h2>School 2: {{ selectedSchools[1].school_name }}</h2>

      <h2> Score: {{ score }}</h2>
      <div>
        <button id="fart" v-if="!gameCont" @click="guess1">School 1</button>
        <button id="fart" v-if="!gameCont"  @click="guess2">School 2</button>
        <button id="fart" v-if="gameCont" @click="continueGame">Begin</button>
      </div>
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>

    <div id="previous-guess" v-if="showGraph">
      <h2>Previous Guess</h2>
      <div id="chart">
        <Bar :data="chartData" :options="chartOptions"/>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, watchEffect } from 'vue';
import { Bar } from 'vue-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

export default {
  name: 'SchoolsDisplay',
  components: { Bar },
  setup() {
    const selectedSchools = ref([]);
    const combinedSATScores = ref([]);

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
    let schools = ref([])

    const fetchData = async () => {
      try {
        const response = await fetch('https://data.cityofnewyork.us/resource/f9bf-2cp4.json');
        const data = await response.json();

        const shuffledData = shuffleArray(data);
        const selected = shuffledData.slice(0, 2);

        data.forEach((el) => {
            schools.value.push(el.school_name)
          })

          const schoolNames = data.reduce((acc, el) => {
            const crash = el.contributing_factor_vehicle_1;
            if (!acc[crash]) {
              acc[crash] = 1;
            } else {
              acc[crash]++;
            }
            return acc;
          }, {});

          const labels = Object.keys(schoolNames);

          chartData.value.labels = labels;
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

  if (selectedSchools.value.length >= 2) {
    const school1Name = selectedSchools.value[0].school_name;
    const school2Name = selectedSchools.value[1].school_name;

    const newData = {
      labels: ['School 1: ' + school1Name, 'School 2: ' + school2Name],
      datasets: [{
        label: 'Combined SAT Scores',
        backgroundColor: '#ff2e77',
        data: combinedSATScores.value
      }]
    };
    chartData.value = newData;
  } else {
  
    const newData = {
      labels: ['', ''],
      datasets: [{
        label: 'Combined SAT Scores',
        backgroundColor: '#ff2e77',
        data: combinedSATScores.value
      }]
    };
    chartData.value = newData;
  }
};


    onMounted(() => {
      fetchData();
    });

    const getCombinedSatScore = (school) => {
      return school.combined_sat_score;
    };

    const score = ref(0)
    const showGraph = ref(false)

    const guess1 = () => {
      if (getCombinedSatScore(selectedSchools.value[0]) > getCombinedSatScore(selectedSchools.value[1])){
        score.value++
        fetchData();
        showGraph.value = true
      } else {
        score.value = 0
        showGraph.value = false
        fetchData(); 
      }
    }

    const guess2 = () => {
      if (getCombinedSatScore(selectedSchools.value[1]) > getCombinedSatScore(selectedSchools.value[0])){
        score.value++
        fetchData();
        showGraph.value = true
      } else {
        score.value = 0
        showGraph.value = false
        fetchData(); 
      }
    }

    watchEffect(() => {
      updateChartData();
    });

    let gameCont = ref(true) 
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
      continueGame,
      showGraph
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
  #previous-guess {
    float: right;
    margin-right: 20px;
  }

  .mainHeader{
    font-family: 'Times New Roman', Times, serif;
    color: white;
    text-decoration: underline;
  }
</style>
