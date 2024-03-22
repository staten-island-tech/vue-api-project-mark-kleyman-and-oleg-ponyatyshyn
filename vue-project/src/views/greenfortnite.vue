<template>
    <div>

    </div>
</template>

<script>
import { ref } from 'vue';
const skibidi = ref([])
const fetchData = async () => {
    try {
        const res = await fetch('https://data.cityofnewyork.us/resource/2rb7-7eqa.json');
        const data = await res.json();
        console.log(data);

        const boroughCount = data.reduce((acc, item) => {
            const borough = item.suspect_race;
            if (!acc[borough]) {
                acc[borough] = 1;
            } else {
                acc[borough]++;
            }
            return acc;
        }, {});
        console.log(boroughCount)
    } catch (error) {
        throw new Error(error)
    }
}
fetchData()
</script>

<style scoped>

</style>