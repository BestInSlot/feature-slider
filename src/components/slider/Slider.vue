<template>
  <section id="featured">
    <div class="featured-container" :style="{ width: `${width}px`, height: `${height}px`}">
      <div class="featured-slides" :style="transitionClasses" @mouseover="isPaused = true" @mouseout="isPaused = false">
        <slide v-for="(slide, i) in featured" :key="i" :slide="slide" :height="height" :width="width"></slide>
      </div>
      <div class="progress-bar">
        <div class="progress" :style="{ width: progress + '%'}"></div>
      </div>
    </div>
    <div class="slide-controls">
        <button class="prev" :disabled="isFirstSlide" @click="prev"></button>
        <button class="next" :disabled="isLastSlide" @click="next"></button>
        <div class="pips">
          <button class="pip" 
          v-for="(n, i) in featured" 
          :key="i" :class="{ active: i === currentSlideIndex }"
          :disabled="i === currentSlideIndex"
          @click="goTo(i)"></button>
        </div>
    </div>
  </section>
</template>

<script>
import Slide from "./Slide";
export default {
  name: "image-slider",
  components: { Slide },
  props: {
    width: {
      type: Number,
      default: 800
    },
    height: {
      type: Number,
      default: 400
    },
    duration: {
      type: Number,
      default: 3000
    },
    slides: {
      type: Array,
      default: () => []
    },
    speed: {
      type: Number,
      default: 600
    },
    autoPlay: {
      type: Boolean,
      default: true
    }
  },

  data() {
    return {
      featured: [],
      currentSlideIndex: 0,
      x: 0,
      transitionDuration: this.speed,
      progress: 0,
      isPaused: false,
      timer: null
    };
  },

  mounted() {
    this.setupSlides();
    this.startTimer();
  },

  beforeDestroy() {
    this.clearTimer();
  },

  computed: {
    transitionClasses() {
      return {
        "transition-duration": `${this.transitionDuration}ms`,
        "transition-timing-function": "cubic-bezier(0.445, 0.05, 0.55, 0.95)",
        transform: `translate3d(${this.x}px, 0, 0)`
      };
    },
    isFirstSlide() {
      return this.currentSlideIndex === 0;
    },
    isLastSlide() {
      return this.currentSlideIndex === this.slides.length - 1;
    }
  },

  watch: {
    currentSlideIndex(n) {
      this.move(n);
    }
  },

  methods: {
    setupSlides() {
      if (this.slides.length) {
        this.featured = this.slides.map((slide, index) => {
          slide.startX = index * this.width;
          return slide;
        });
      }
    },

    startTimer() {
      if (this.autoPlay) {
        if (!this.timer) {
          this.timer = setInterval(() => {
            if (this.isPaused) return;
            this.progress++;
            if (this.progress >= 100) {
              if (!this.isLastSlide) {
                this.currentSlideIndex++;
                // this.move(this.currentSlideIndex);
              } else {
                this.currentSlideIndex = 0;
                // this.move(this.currentSlideIndex);
              }
            }
          }, this.duration / 100);
        }
      }
    },

    clearTimer() {
      clearInterval(this.timer);
      this.progress = 0;
      this.timer = null;
    },

    resetTimer() {
      this.clearTimer();
      const timeout = setTimeout(() => {
        this.$nextTick(() => {
          this.startTimer();
          clearTimeout(timeout);
        });
      }, this.speed);
    },

    next() {
      if (this.isLastSlide) return;
      this.currentSlideIndex++;
      // this.move(this.currentSlideIndex);
    },

    prev() {
      if (this.isFirstSlide) return;
      this.currentSlideIndex--;
      // this.move(this.currentSlideIndex);
    },

    goTo(n) {
      this.currentSlideIndex = n;
      // this.move(this.currentSlideIndex);
    },

    move(i) {
      this.x = -Math.abs(i * this.width);
      if (this.timer) {
        this.resetTimer();
      }
    }
  }
};
</script>

<style lang="scss">
#featured {
  position: relative;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.7);
  .prev,
  .next {
    position: absolute;
    height: 90px;
    width: 90px;
    background-color: rgba(0, 0, 0, 0.7);
    top: 35%;
    border: none;
    &:focus {
      outline: none;
    }
  }
  .prev {
    left: -90px;
    border-top-left-radius: 4px;
    border-bottom-left-radius: 4px;
  }
  .next {
    right: -90px;
    border-top-right-radius: 4px;
    border-bottom-right-radius: 4px;
  }
  .pips {
    position: absolute;
    bottom: -15px;
    left: 0;
    right: 0;
    .pip {
      display: inline-block;
      width: 20px;
      padding: 5px;
      background-color: #555;
      border: 0;
      border-radius: 1px;
      margin-right: 2px;
      &:last-child {
        margin-right: 0;
      }
      &.active {
        background-color: red;
      }
    }
  }
  .featured-container {
    width: 100%;
    height: 100%;
    overflow: hidden;
    position: relative;
    .featured-slides {
      position: relative;
      .slide {
        position: absolute;
        z-index: 1000;
      }
    }
    .progress-bar {
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      height: 3px;
      background-color: #555;
      .progress {
        width: inherit;
        height: inherit;
        background-color: red;
      }
    }
  }
}
</style>
