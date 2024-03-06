<template>
  <div>

  </div>
</template>

<script setup>
import {ref, onMounted} from 'vue'
const crashes = ref("")
async function getCrashes() {
  let res = await fetch ("https://data.cityofnewyork.us/resource/h9gi-nx95.json")
  let data = await res.json();
  console.log(data);
  crashes.value = data.results
}

onMounted(() => {
getCrashes()
});


</script>

<style lang="scss" scoped>

</style>