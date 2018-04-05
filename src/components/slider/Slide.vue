<template>
  <div class="slide" :style="{ left: slide.startX + 'px', width: width + 'px', height: height + 'px' }">
    <img :src="slide.img" alt="" style="width: 100%; height: 100%;">
    <div class="desc"></div>  
  </div>
</template>

<script>
export default {
  props: {
    slide: {
      type: Object,
      required: true
    },
    height: {
      type: Number,
      default: 400
    },
    width: {
      type: Number,
      default: 800
    }
  }
};
</script>
