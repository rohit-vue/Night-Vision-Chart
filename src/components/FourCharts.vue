<template>
  <div class="h-98-perc">
    <div class="w-full h-half flex mb-2px">
      <div id="chart-container-1" class="w-half mr-2px"></div>
      <div id="chart-container-2" class="w-half"></div>
    </div>
    <div class="w-full h-half flex">
      <div id="chart-container-3" class="w-half mr-2px"></div>
      <div id="chart-container-4" class="w-half"></div>
    </div>
  </div>
</template>

<script>
import { NightVision } from 'night-vision';
import data1 from './data-1.json';
import data2 from './data-2.json';


export default {
  name: 'FourChats',
  data() {
    return {
      chart1: null,
      chart2: null,
      chart3: null,
      chart4: null,
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

      this.chart2 = new NightVision('chart-container-2', {
        id: 'nvjs-2',
        data: data2,
        autoResize: true,
        colors: { back: '#111113', grid: '#2e2f3055' },
      });
      
      this.chart3 = new NightVision('chart-container-3', {
        id: 'nvjs-3',
        data: data2,
        autoResize: true,
        colors: { back: '#111113', grid: '#2e2f3055' },
      });

      this.chart4 = new NightVision('chart-container-4', {
        id: 'nvjs-4',
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

      this.chart2.events.on('app:$range-update', (range) => {
        this.chart2.range = range;
      });

      this.chart3.events.on('app:$range-update', (range) => {
        this.chart3.range = range;
      });

      this.chart4.events.on('app:$range-update', (range) => {
        this.chart4.range = range;
      });

      // Cursor
      this.chart1.events.on('app:$cursor-update', (cursor) => {
        this.chart1.cursor = cursor;
      });

      this.chart2.events.on('app:$cursor-update', (cursor) => {
        this.chart2.cursor = cursor;
      });
      
      this.chart3.events.on('app:$cursor-update', (cursor) => {
        this.chart3.cursor = cursor;
      });
      
      this.chart4.events.on('app:$cursor-update', (cursor) => {
        this.chart4.cursor = cursor;
      });
    },
  },
};
</script>

<style scoped>
</style>
