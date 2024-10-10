<template>
  <div>
    <canvas ref="canvas" width="800" height="600"></canvas>
    <div>
      <label for="timeScale">Time Scale: </label>
      <input type="range" id="timeScale" v-model="timeScale" min="1" max="10" />
      <label for="priceScale">Price Scale: </label>
      <input type="range" id="priceScale" v-model="priceScale" min="1" max="10" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timeScale: 1,
      priceScale: 1,
      canvas: null,
      ctx: null,
    };
  },
  mounted() {
    this.canvas = this.$refs.canvas;
    this.ctx = this.canvas.getContext('2d');
    this.drawFibonacciCircle();
  },
  watch: {
    timeScale: 'drawFibonacciCircle',
    priceScale: 'drawFibonacciCircle',
  },
  methods: {
    drawFibonacciCircle() {
      const centerX = this.canvas.width / 2;
      const centerY = this.canvas.height / 2;
      const baseRadius = 50;

      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height); // Clear the canvas

      // Draw Fibonacci Circles
      for (let i = 0; i < 10; i++) {
        const fib = this.fibonacci(i);
        const radius = baseRadius * fib * this.timeScale;
        this.ctx.beginPath();
        this.ctx.arc(centerX, centerY, radius, 0, 2 * Math.PI);
        this.ctx.strokeStyle = 'rgba(0, 128, 0, 0.5)';
        this.ctx.lineWidth = 2;
        this.ctx.stroke();
      }
    },
    fibonacci(n) {
      if (n <= 1) return n;
      return this.fibonacci(n - 1) + this.fibonacci(n - 2);
    },
  },
};
</script>

<style scoped>
canvas {
  border: 1px solid black;
}
</style>
