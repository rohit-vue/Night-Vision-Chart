<template>
  <div>
    <div :id="id"></div>
    <div class="button-group">
      <button @click="togglePane">{{ paneButtonLabel }}</button>
      <button @click="toggleOverlay">{{ overlayButtonLabel }}</button>
      <button @click="toggleOverlayVisibility">{{ overlayVisibilityButtonLabel }}</button>
    </div>
  </div>
</template>

<script>
import { NightVision } from 'night-vision';
import data from './data-1.json';
import ovData1 from './overlay-data-1.json';
import ovData2 from './overlay-data-2.json';

export default {
  name: 'NightVisionChart2',
  data() {
    return {
      chart: null,
      overlayVisible: true,
    };
  },
  props: {
    id: {
      type: String,
      required: true,
    },
  },
  computed: {
    paneButtonLabel() {
      // console.log(object)
      return this.chart && this.chart?.data.panes.length > 1 ? 'Remove Pane' : 'Add Pane';
    },
    overlayButtonLabel() {
      const overlays = this.chart?.data.panes[0]?.overlays || [];
      return overlays.length > 2 ? 'Remove Overlay' : 'Add Overlay';
    },
    overlayVisibilityButtonLabel() {
      return this.overlayVisible ? 'Hide Overlays' : 'Show Overlays';
    },
  },
  mounted() {
    this.initializeChart();
  },
  methods: {
    initializeChart() {
      this.chart = new NightVision(this.id, {
        data,
        autoResize: true,
        colors: {
          back: '#111113',
          grid: '#2e2f3055',
        },
      });
    },
    togglePane() {
      if (this.chart.data.panes.length <= 1) {
        this.chart.data.panes.push({
          settings: {},
          overlays: [ovData2],
        });
      } else {
        this.chart.data.panes.pop();
      }
      this.chart.update();
    },
    toggleOverlay() {
      const overlays = this.chart.data.panes[0].overlays;
      if (overlays.length <= 2) {
        overlays.push(ovData1);
      } else {
        overlays.pop();
      }
      this.chart.update();
    },
    toggleOverlayVisibility() {
      this.overlayVisible = !this.overlayVisible;
      this.chart.hub.allOverlays().forEach((overlay, index) => {
        if (index === 0) return; // Skip the main overlay
        overlay.settings.display = this.overlayVisible;
      });
      this.chart.update();
      this.chart.update('legend');
    },
  },
};
</script>

<style scoped>
/* Add any additional styling here */
.button-group {
  margin-top: 10px;
  margin-bottom: 30px;
}
</style>
