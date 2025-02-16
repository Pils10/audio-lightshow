<template>
  <div class="projector-visualizer">
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
export default {
  props: {
    audioData: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      canvas: null,
      ctx: null
    };
  },
  mounted() {
    this.canvas = this.$refs.canvas;
    this.ctx = this.canvas.getContext('2d');
    this.resizeCanvas();
    window.addEventListener('resize', this.resizeCanvas);
    this.animate();
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.resizeCanvas);
  },
  methods: {
    resizeCanvas() {
      this.canvas.width = window.innerWidth;
      this.canvas.height = window.innerHeight;
    },
    animate() {
      requestAnimationFrame(this.animate);
      this.drawProjectorBeams();
    },
    drawProjectorBeams() {
      const { ctx, canvas, audioData } = this;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      const beamCount = 10;
      const beamWidth = canvas.width / beamCount;
      const maxBeamHeight = canvas.height;
      for (let i = 0; i < beamCount; i++) {
        const beamHeight = (audioData[i] / 255) * maxBeamHeight;
        ctx.fillStyle = `rgba(255, 255, 255, ${audioData[i] / 255})`;
        ctx.fillRect(i * beamWidth, canvas.height - beamHeight, beamWidth, beamHeight);
      }
    }
  }
};
</script>

<style scoped>
.projector-visualizer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
}
canvas {
  display: block;
}
</style>
