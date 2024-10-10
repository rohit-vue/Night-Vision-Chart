<template>
  <div>
    <div class="h-91vh">
      <div class="p-5 flex gap-2">
        <button @click="screens = 1">Split to one</button>
        <button @click="screens = 2">Split to two</button>
        <button @click="screens = 3">Split to three</button>
        <button @click="screens = 4">Split to four</button>
      </div>
      <div v-if="screens == 1" class="h-full">
        <OneChart />
      </div>
      <div v-else-if="screens == 2" class="h-full">
        <TwoCharts />
      </div>
      <div v-else-if="screens == 3" class="h-98-perc">
        <ThreeCharts />
      </div>
      <div v-else-if="screens == 4" class="h-98-perc">
        <FourCharts />
      </div>
    </div>
  </div>
</template>

<script>
import OneChart from './OneChart.vue';
import TwoCharts from './TwoCharts.vue';
import ThreeCharts from './ThreeCharts.vue';
import FourCharts from './FourCharts.vue';

export default {
  data() {
    return {
      screens: 1,
    };
  },
  components: {
    OneChart,
    TwoCharts,
    ThreeCharts,
    FourCharts,
  },
};

</script>

<style scoped>

</style>
