<template>
  <div>
    <div class="flex gap-2 w-full justify-between">
      <!-- <div :id="id"></div> -->
      <div id="chart-container-1"></div>
      <div id="chart-container-2"></div>
    <!-- <NightVisionChart2 /> -->

    </div>
  </div>
</template>

<script>
import { NightVision } from 'night-vision';
// import NightVisionChart2 from './NightVisionChart2.vue';
import data1 from './data-1.json';
import data2 from './data-2.json';

export default {
  name: 'DualNightVisionChart',
  data() {
    return {
      chart1: null,
      chart2: null,
    };
  },
  // props: {
  //   id: {
  //     type: String,
  //     required: true,
  //   },
  // },
  components: {
    // NightVisionChart2
  },
  mounted() {
    this.initializeCharts();
    this.setupEventSync();
  },
  methods: {
    initializeCharts() {
      // this.chart1 = new NightVision(this.id, {
        this.chart1 = new NightVision('chart-container-1', {
        id: 'nvjs-1',
        data: data1,
        autoResize: true,
        colors: { back: '#1b1b1c', grid: '#2e2f3099' },
      });

      this.chart2 = new NightVision('chart-container-2', {
        id: 'nvjs-2',
        data: data2,
        autoResize: true,
        colors: { back: '#111113', grid: '#2e2f3055' },
      });
    },
    setupEventSync() {
      // Time-range sync
      // this.chart1.events.on('app:$range-update', (range) => {
      //   this.chart2.range = range;
      // });
      this.chart1.events.on('app:$range-update', (range) => {
        this.chart1.range = range;
      });

      // this.chart2.events.on('app:$range-update', (range) => {
      //   this.chart1.range = range;
      // });
      this.chart2.events.on('app:$range-update', (range) => {
        this.chart2.range = range;
      });
      // Cursor sync
      // this.chart1.events.on('app:$cursor-update', (cursor) => {
      //   this.chart2.cursor = cursor;
      // });
      this.chart1.events.on('app:$cursor-update', (cursor) => {
        this.chart1.cursor = cursor;
      });

      // this.chart2.events.on('app:$cursor-update', (cursor) => {
      //   this.chart1.cursor = cursor;
      // });
      this.chart2.events.on('app:$cursor-update', (cursor) => {
        this.chart2.cursor = cursor;
      });
    },
  },
};
</script>

<style scoped>
/* Add any additional styling here */
.flex {
  display: flex;
}
</style>
