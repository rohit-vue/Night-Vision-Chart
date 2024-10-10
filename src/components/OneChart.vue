<template>
  <div class="h-full">
    <div class="w-full h-full">
      <div id="chart-container-1" class="h-full"></div>
    </div>
  </div>
</template>

<script>
import { NightVision } from 'night-vision';
import data1 from './data-1.json';

export default {
  name: 'OneChart',
  data() {
    return {
      chart1: null,
    };
  },
  mounted() {
    this.initializeCharts();
    this.setupEventSync();
  },
  methods: {
    initializeCharts() {
      this.chart1 = new NightVision('chart-container-1', {
        id: 'nvjs-1',
        data: data1,
        autoResize: true,
        colors: { back: '#1b1b1c', grid: '#2e2f3099' },
      });
    },
    setupEventSync() {
      // Time-range 
      this.chart1.events.on('app:$range-update', (range) => {
        this.chart1.range = range;
      });

      // Cursor
      this.chart1.events.on('app:$cursor-update', (cursor) => {
        this.chart1.cursor = cursor;
      });
    },
  },
};
</script>

<style scoped>
</style>
