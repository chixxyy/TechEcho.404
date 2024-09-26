<template>
  <div ref="canvasContainer"></div>
</template>

<script>
import p5 from 'p5';
let bgColor;

export default {
  name: 'P5Canvas',
  mounted() {
    new p5(this.sketch, this.$refs.canvasContainer);
  },
  methods: {
    sketch(p) {
      p.setup = () => {
        p.createCanvas(p.windowWidth, p.windowHeight, p.WEBGL);
        p.angleMode(p.DEGREES);
        p.strokeWeight(5);
        p.noFill();
        p.stroke(32, 12, 64);
        p.describe('這是一座隱密的高塔');

        bgColor = p.color(p.random(200, 255), p.random(180, 240), p.random(200, 255));
      };

      p.draw = () => {
        p.background(bgColor);

        p.orbitControl();

        for (let zAngle = 0; zAngle < 180; zAngle += 30) {
          for (let xAngle = 0; xAngle < 360; xAngle += 30) {
            p.push();
            p.rotateZ(zAngle);
            p.rotateX(xAngle);
            p.translate(0, 400, 0);
            p.box();
            p.pop();
          }
        }
      };

      p.windowResized = () => {
        p.resizeCanvas(p.windowWidth, p.windowHeight);
      };
    }
  }
};
</script>

<style scoped>
canvas {
  display: block;
}
</style>
