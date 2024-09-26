<template>
  <div class="relative">
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap" rel="stylesheet">
    <div ref="p5Canvas" class="p5-container"></div>
    <div class="absolute z-10 redirect-message top-10 right-10">
      <p class="text-3xl text-red-500 redirect-link">
        訪問的頁面不存在，5秒後
        <button 
          class="bg-orange-500 hover:bg-orange-700 text-white font-bold p-2 m-2 rounded"
          onclick="window.location.href='https://www.tech-echo.dev/'"
        >
          跳回首頁
        </button>
      </p>
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

    
    setTimeout(() => {
      window.location.href = "https://www.tech-echo.dev/";
    }, 5000); 
  },
  methods: {
    createSketch(p) {
      const particles = [];
      const txt = "404";
      const clr = "white";
      let txtindex = 0;

      let res;
      let scale;
      let margin;
      let xSpacing; 
      let ySpacing; 
      let maxDist;

      const speed = 0.015;

      function reset() {
        scale = res / 60;
        margin = 30 * scale;
        xSpacing = 20 * scale;
        ySpacing = 20 * scale;
        maxDist = 100 * scale;
      }

      p.setup = function() {
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
              txt: txt[txtindex]
            });

            txtindex = (txtindex + 1) % txt.length;
          }
        }

        this.isLoading = false;
      }.bind(this);

      p.draw = function() {
        p.background(0);

        for (let particle of particles) {
          const d = p.dist(particle.origX, particle.origY, p.mouseX, p.mouseY);
          const distortionX = p.map(d, 0, maxDist, p.mouseX, particle.origX, true);
          const distortionY = p.map(d, 0, maxDist, p.mouseY, particle.origY, true);

          particle.targetX = distortionX;
          particle.targetY = distortionY;

          const dx = (particle.targetX - particle.x) * speed;
          const dy = (particle.targetY - particle.y) * speed;
          particle.x += dx;
          particle.y += dy;

          p.textSize(p.map(d, 0, maxDist, 2 * scale, 16 * scale, true));
          p.fill(particle.clr);
          p.text(particle.txt, particle.x, particle.y);
        }
      };

      p.windowResized = function() {
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
