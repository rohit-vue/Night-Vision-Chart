<template>
  <div class="h-full">
    <div class="w-full h-full">
      <div id="chart-container-1" class="h-full"></div>
      <button @click="startFibonacciCircle">Start Fibonacci Circle</button>
      <button @click="cancelFibonacciCircle">Cancel</button>
      <div v-if="showFibonacciCircle" class="fibonacci-circle">Fibonacci Circle</div>
    </div>
  </div>
</template>

<script>
import { NightVision } from 'night-vision';
import data1 from '../components/data-1.json';

export default {
  name: 'OneChart',
  data() {
    return {
      chart1: null,
      showFibonacciCircle: false,
      isDragging: false,
      circle: {
        x: 0,
        y: 0,
        radius: 0,
      },
    };
  },
  mounted() {
    this.initializeCharts();
    this.setupEventSync();
    this.drawPredefinedCircles();
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
    drawPredefinedCircles() {
      if (data1.circles) {
        data1.circles.forEach(circle => {
          this.drawFibonacciCircle(circle.x, circle.y, circle.radius);
        });
      }
    },
    startFibonacciCircle() {
      this.showFibonacciCircle = true;
      document.getElementById('chart-container-1').addEventListener('click', this.toggleFibonacciCircle);
    },
    cancelFibonacciCircle() {
      this.showFibonacciCircle = false;
      this.removeFibonacciCircle();
      this.saveCircleCoordinates();
    },
    toggleFibonacciCircle(event) {
      if (!this.showFibonacciCircle) return;

      this.startX = event.clientX;
      this.startY = event.clientY;
      this.isDragging = true;

      document.addEventListener('mousemove', this.onMouseMove);
      document.addEventListener('mouseup', this.onMouseUp);
    },
    onMouseMove(event) {
      if (!this.isDragging) return;

      const { clientX, clientY } = event;
      const radius = Math.sqrt(
        Math.pow(clientX - this.startX, 2) + Math.pow(clientY - this.startY, 2)
      );

      this.circle = { x: this.startX, y: this.startY, radius };
      this.drawFibonacciCircle(this.circle.x, this.circle.y, this.circle.radius);
    },
    onMouseUp() {
      this.isDragging = false;
      document.removeEventListener('mousemove', this.onMouseMove);
      document.removeEventListener('mouseup', this.onMouseUp);
    },
    drawFibonacciCircle(x, y, radius) {
      let canvas = document.getElementById('fibonacci-circle-canvas');

      if (!canvas) {
        canvas = document.createElement('canvas');
        canvas.id = 'fibonacci-circle-canvas';
        document.getElementById('chart-container-1').appendChild(canvas);
      }

      canvas.width = radius * 2;
      canvas.height = radius * 2;
      canvas.style.position = 'absolute';
      canvas.style.left = `${x - radius}px`;
      canvas.style.top = `${y - radius}px`;
      canvas.style.zIndex = '10'; // Ensure it's on top

      const ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, radius * 2, radius * 2);

      ctx.beginPath();
      ctx.arc(radius, radius, radius, 0, 2 * Math.PI);
      ctx.fillStyle = 'rgba(255, 255, 255, 0.3)';
      ctx.fill();
      ctx.strokeStyle = 'white';
      ctx.stroke();

      this.drawFibonacciLines(ctx, radius);
      this.addResizeHandles(canvas, radius);
    },
    addResizeHandles(canvas, radius) {
      const handleSize = 8;
      const ctx = canvas.getContext('2d');
      ctx.fillStyle = 'red';

      // Draw handles
      ctx.fillRect(radius * 2 - handleSize / 2, radius - handleSize / 2, handleSize, handleSize);
      ctx.fillRect(radius - handleSize / 2, radius * 2 - handleSize / 2, handleSize, handleSize);

      // Add event listeners for resizing
      canvas.addEventListener('mousedown', this.onHandleMouseDown);
    },
    onHandleMouseDown(event) {
      // Logic to handle resizing
      // Determine which handle is being dragged and update the circle's radius accordingly
    },
    drawFibonacciLines(ctx, radius) {
      // Fibonacci levels for demonstration
      const levels = [0.236, 0.382, 0.618, 1, 1.618];
      ctx.strokeStyle = 'yellow';
      ctx.lineWidth = 2;

      levels.forEach(level => {
        const yPos = radius - radius * level; // Calculate the position based on level
        ctx.beginPath();
        ctx.moveTo(0, yPos);
        ctx.lineTo(radius * 2, yPos); // Draw horizontal lines across the circle
        ctx.stroke();
      });
    },
    removeFibonacciCircle() {
      const canvas = document.getElementById('fibonacci-circle-canvas');
      if (canvas) {
        canvas.remove();
      }
      console.log('Removing Fibonacci Circle');
    },
    saveCircleCoordinates() {
      const circleData = {
        x: this.circle.x,
        y: this.circle.y,
        radius: this.circle.radius,
      };

      // Simulate saving to data-1.json by updating the in-memory data
      data1.circles = data1.circles || [];
      data1.circles.push(circleData);

      console.log('Simulated saving circle coordinates:', circleData);

      // Reinitialize the chart with updated data
      this.reinitializeChart();
    },
    reinitializeChart() {
      // Check if the chart instance exists and has a destroy method
      if (this.chart1 && typeof this.chart1.destroy === 'function') {
        this.chart1.destroy();
        console.warn('Chart destroyed');
      } else {
        console.warn('Chart instance does not exist or destroy method is not available.');
      }

      // Create a new chart instance with the updated data
      this.chart1 = new NightVision('chart-container-1', {
        id: 'nvjs-1',
        data: data1,
        autoResize: true,
        colors: { back: '#1b1b1c', grid: '#2e2f3099' },
      });

      console.log('Chart reinitialized with updated data:', data1);
    },
  },
};
</script>

<style scoped>
.fibonacci-circle {
  position: absolute; /* Adjust positioning as needed */
  /* Add any styling for the Fibonacci Circle here */
}
</style>