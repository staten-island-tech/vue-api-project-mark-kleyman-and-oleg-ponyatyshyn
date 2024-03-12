<template>
  <div>
    <BarChart/>
  </div>
</template>

<script setup>

import BarChart from '@/components/BarChart.vue'

import {ref, onBeforeMount} from 'vue'
const crashes = []
async function getCrashes() {
  let res = await fetch ("https://data.cityofnewyork.us/resource/h9gi-nx95.json")
  let data = await res.json();
  data.forEach((item)=>{
   crashes.push(item.contributing_factor_vehicle_1)
   crashes.sort()
  })
  function onlyUnique(value, index, array) {
    return array.indexOf(value) === index;
  }
  const unique = crashes.filter(onlyUnique)
  console.log(unique)
}

onBeforeMount(() => {
  getCrashes()
});
</script>

<style lang="scss" scoped>

</style>