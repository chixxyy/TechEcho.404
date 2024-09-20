<template>
  <div class="scene"> 
  
  <div class="back"></div>
  
  <div class="slider" style="--t: 0">
    <i style="--i: 0"></i>
    <i style="--i: 1"></i>
    <i style="--i: 2"></i>
    <i style="--i: 3"></i>
    <i style="--i: 4"></i>
    <i style="--i: 5"></i>
  </div>
  <div class="slider" style="--t: 1">
    <i style="--i: 0"></i>
    <i style="--i: 1"></i>
    <i style="--i: 2"></i>
    <i style="--i: 3"></i>
    <i style="--i: 4"></i>
    <i style="--i: 5"></i>
  </div>
  <div class="slider" style="--t: 2">
    <i style="--i: 0"></i>
    <i style="--i: 1"></i>
    <i style="--i: 2"></i>
    <i style="--i: 3"></i>
    <i style="--i: 4"></i>
    <i style="--i: 5"></i>
  </div>
  <div class="slider" style="--t: 3">
    <i style="--i: 0"></i>
    <i style="--i: 1"></i>
    <i style="--i: 2"></i>
    <i style="--i: 3"></i>
    <i style="--i: 4"></i>
    <i style="--i: 5"></i>
  </div>
  <div class="slider" style="--t: 4">
    <i style="--i: 0"></i>
    <i style="--i: 1"></i>
    <i style="--i: 2"></i>
    <i style="--i: 3"></i>
    <i style="--i: 4"></i>
    <i style="--i: 5"></i>
  </div>
  <div class="slider" style="--t: 5">
    <i style="--i: 0"></i>
    <i style="--i: 1"></i>
    <i style="--i: 2"></i>
    <i style="--i: 3"></i>
    <i style="--i: 4"></i>
    <i style="--i: 5"></i>
  </div>
  <div class="slider" style="--t: 6">
    <i style="--i: 0"></i>
    <i style="--i: 1"></i>
    <i style="--i: 2"></i>
    <i style="--i: 3"></i>
    <i style="--i: 4"></i>
    <i style="--i: 5"></i>
  </div>
  <div class="slider" style="--t: 7">
    <i style="--i: 0"></i>
    <i style="--i: 1"></i>
    <i style="--i: 2"></i>
    <i style="--i: 3"></i>
    <i style="--i: 4"></i>
    <i style="--i: 5"></i>
  </div>
  <div class="slider" style="--t: 8">
    <i style="--i: 0"></i>
    <i style="--i: 1"></i>
    <i style="--i: 2"></i>
    <i style="--i: 3"></i>
    <i style="--i: 4"></i>
    <i style="--i: 5"></i>
  </div>
  <div class="slider" style="--t: 9">
    <i style="--i: 0"></i>
    <i style="--i: 1"></i>
    <i style="--i: 2"></i>
    <i style="--i: 3"></i>
    <i style="--i: 4"></i>
    <i style="--i: 5"></i>
  </div>
  
  <div class="ball">
    <div class="innerBall"></div>
  </div>

  <div class="error-message">
    <h1>404</h1>
    <a href="https://www.tech-echo.dev/">回首頁
    </a>
  </div>
</div>
</template>

<style scoped>
*, *::before, *::after {
    padding: 0;
    margin: 0 auto;
    box-sizing: border-box;
}

body {
  background-color: #aaa;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  perspective: 200vh;
  overflow: hidden; 
  
  * {
    transform-style: preserve-3d;
  }
}

.scene {
  --duration: 16s;
  position: relative;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: scene calc(var(--duration) * 2) infinite linear;
}

@keyframes scene {
  from { transform: rotate(0deg) rotateX(30deg) rotate(360deg); }
  to { transform: rotate(360deg) rotateX(30deg) rotate(0deg); }
}

.back {
  position: absolute;
  inset: -200vh -40vh;
  background-color: #f00;
  transform: translateZ(-8vh);
  background:
    linear-gradient(to right, #aaa, transparent, #aaa),
    repeating-linear-gradient(#ddd 0 1vh, transparent 0 9.375vh) 0 0 / 100% 100%,
    conic-gradient(from 270deg at 1vh 50%, #ddd 0 90deg, transparent 0) 2vh 0 / 18.75vh 18.75vh,
    conic-gradient(from 270deg at 1vh 50%, #ddd 0 90deg, transparent 0) 11.375vh 9.375vh / 18.75vh 18.75vh;
  animation: sliderY var(--duration) infinite linear;
}

.slider {
  --delay: calc(var(--duration) / -10 * var(--t));
  
  position: absolute;
  animation: sliderY var(--duration) var(--delay) infinite linear;
  
  i {
    --border-color: hsl(calc(var(--t) * 36) 75% calc(var(--i) * 5% + 25%));
    
    position: absolute;
    inset: -12vh -8vh;
    border-radius: 50%;
    border: 1vh solid;
    transform: translateZ(calc(var(--i) * 0.3vh + 1vh));
    
    &::after {
      content: '';
      position: absolute;
      inset: -1vh;
      border: inherit;
      border-radius: inherit;
      transform: translateZ(-3.5vh);
    }
  }
  &::after {
    --border-color: #000;

    content: '';
    position: absolute;
    inset: -12vh -8vh;
    border-radius: 50%;
    border: 1vh solid;
    border-color: transparent transparent #000 #000;
    transform: translateZ(-8vh);
    filter: blur(1vh);
  }
  
  &:nth-child(odd) {
    --translateX: 10vh;

    i, &::after {
      border-color: transparent var(--border-color) var(--border-color) transparent;
    }
  }
  &:nth-child(even) {
    --translateX: -10vh;
    
    i, &::after {
      border-color: transparent transparent var(--border-color) var(--border-color);
    }
  }
}

@keyframes sliderY {
  from { translate: var(--translateX, 0) 93.75vh; }
  to { translate: var(--translateX, 0) -93.75vh; }
}

.ball {
  position: absolute;
  animation:
    ballX calc(var(--duration) / 5) infinite ease-in-out,
    ballY calc(var(--duration) / 10) calc(var(--duration) * -9.975) infinite;
  
  &::before {
    content: '';
    position: absolute;
    inset: -2vh;
    border-radius: 50%;
    background-color: #0007;
    transform: translateZ(-8vh);
    filter: blur(1vh);
  }
}

@keyframes ballX {
  0%, 100% { translate: 16vh; }
  50% { translate: -16vh; }
}

@keyframes ballY {
  0%, 100%{ transform: translateY(5vh); animation-timing-function: cubic-bezier(.3,.0,.58,1); }
  36% { transform: translateY(-5vh); animation-timing-function: cubic-bezier(.42,0,.7,1); }
}

.innerBall {
  position: absolute;
  inset: -2vh;
  border-radius: 50%;
  background-image: radial-gradient(circle at 50% 15%, #fff, #000);
  animation: innerBall calc(var(--duration) * 2) infinite linear;
}

@keyframes innerBall {
  from { transform: rotate(0deg) rotateX(-30deg) rotate(360deg); }
  to { transform: rotate(360deg) rotateX(-30deg) rotate(0deg); }
}

.error-message {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: red;
  z-index: 100; 
}

.error-message h1 {
  font-size: 15vh;
}
</style>

<script>
export default {
  mounted() {
    setTimeout(() => {
      window.location.href = 'https://www.tech-echo.dev/';
    }, 5000);
  }
}
</script>
