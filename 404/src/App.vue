<template>
  <div class="relative">
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
    <div ref="p5Canvas" class="p5-container"></div>
    <div class="absolute z-10 redirect-message top-10 right-10">
      <p class="text-3xl text-red-500 redirect-link">
        訪問的頁面不存在，5秒後
        <button 
          class="p-2 m-2 font-bold text-white bg-orange-500 rounded hover:bg-orange-700"
          @click="redirectToHome"
        >
          跳回首頁
        </button>
      </p>
      <p v-if="isLoading" class="text-lg">加載中...</p> 
    </div>
  </div>
</template>

<script>
import p5 from 'p5';

export default {
  name: 'BreatheAnimation',
  data() {
    return {
      isLoading: true,
    };
  },
  mounted() {
    this.sketch = new p5(this.createSketch, this.$refs.p5Canvas);
    
    setTimeout(this.redirectToHome, 5000); 
  },
  methods: {
    redirectToHome() {
      window.location.href = "https://www.tech-echo.dev/";
    },
    createSketch(p) {
      const particles = [];
      const txt = "404";
      const clr = "white";
      const speed = 0.015;
      let res, scale, margin, xSpacing, ySpacing, maxDist;

      const reset = () => {
        scale = res / 60;
        margin = 30 * scale;
        xSpacing = 20 * scale;
        ySpacing = 20 * scale;
        maxDist = 100 * scale;
      };

      p.setup = () => {
        res = Math.min(p.windowWidth, p.windowHeight);
        reset();
        p.createCanvas(p.windowWidth, p.windowHeight);
        p.angleMode(p.DEGREES);
        p.textAlign(p.CENTER, p.CENTER);
        p.textFont("Orbitron");

        for (let y = margin; y <= p.height - margin; y += ySpacing) {
          for (let x = margin; x <= p.width - margin; x += xSpacing) {
            particles.push({
              x,
              y,
              clr,
              origX: x,
              origY: y,
              targetX: x,
              targetY: y,
              txt: txt[(y / ySpacing + x / xSpacing) % txt.length]
            });
          }
        }

        this.isLoading = false;
      };

      p.draw = () => {
        p.background(0);

        particles.forEach(particle => {
          const d = p.dist(particle.origX, particle.origY, p.mouseX, p.mouseY);
          const distortionX = p.map(d, 0, maxDist, p.mouseX, particle.origX, true);
          const distortionY = p.map(d, 0, maxDist, p.mouseY, particle.origY, true);

          particle.targetX = distortionX;
          particle.targetY = distortionY;

          particle.x += (particle.targetX - particle.x) * speed;
          particle.y += (particle.targetY - particle.y) * speed;

          p.textSize(p.map(d, 0, maxDist, 2 * scale, 16 * scale, true));
          p.fill(particle.clr);
          p.text(particle.txt, particle.x, particle.y);
        });
      };

      p.windowResized = () => {
        p.resizeCanvas(p.windowWidth, p.windowHeight);
        reset();
      };
    }
  },
  beforeUnmount() {
    if (this.sketch) {
      this.sketch.remove();
    }
  }
};
</script>

<style scoped>
.p5-container {
  width: 100%;
  height: 100vh;
}
</style>
