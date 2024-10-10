<template>
  <div>
    <h1>Night Vision Charts</h1>
    <div id="chart-container"></div>
  </div>
</template>

<script>
import { NightVision } from 'night-vision';
// import './style.css'; // Make sure to include your CSS if needed

export default {
  name: 'NightVisionChart',
  mounted() {
    // Generate some random data
    const generateData = () => {
      return Array(30).fill(1).map((_, i) => [
        new Date(`${i + 1} Nov 2022 GMT+0000`).getTime(),
        i * Math.random(),
      ]);
    };

    // Initialize the chart
    const chart = new NightVision('chart-container');
    chart.data = {
      panes: [
        {
          overlays: [
            {
              name: 'APE Stock',
              type: 'Spline',
              data: generateData(),
              settings: {
                precision: 2,
              },
            },
          ],
        },
      ],
    };
  },
};
</script>
<!-- body {
  background-color: #0c0d0e;
} -->
<style scoped>
/* Add any additional styling here */
</style>
