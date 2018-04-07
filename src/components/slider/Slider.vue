<template>
  <section id="featured">
    <div class="featured-container" ref="slider" :style="{ 'width': `${width}px`, height: `${height}px`}">
      <transition-group  tag="div" class="featured-slides" name="fade">
        <slide v-for="(slide, i) in featured" :key="i" 
        :slide="slide" :height="height" 
        :width="width"
        @mouseover.native="isPaused = true" 
        @mouseout.native="isPaused = false" 
        v-show="currentSlideIndex === i"></slide>
      </transition-group>
      <div class="progress-bar">
        <div class="progress" :style="{ width: progress + '%'}"></div>
      </div>
    </div>
    <div class="slide-controls">
        <button class="prev" :disabled="isFirstSlide" @click="prev">&#60;</button>
        <button class="next" :disabled="isLastSlide" @click="next">&#62;</button>
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
              } else {
                this.currentSlideIndex = 0;
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
    },

    prev() {
      if (this.isFirstSlide) return;
      this.currentSlideIndex--;
    },

    goTo(n) {
      this.currentSlideIndex = n;
    },

    move(i) {
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
    font-size: 175%;
    font-weight: 700;
    color: #cacaca;
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
        /* z-index: 1000; */
      }
    }
    .progress-bar {
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      height: 3px;
      background-color: #555;
      z-index: 1000;
      .progress {
        width: inherit;
        height: inherit;
        background-color: red;
      }
    }
  }
}

.fade-enter-active {
  animation: fadeIn 600ms ease-in forwards;
}

.fade-leave-to {
  animation: fadeOut 600ms ease-in forwards;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
</style>
