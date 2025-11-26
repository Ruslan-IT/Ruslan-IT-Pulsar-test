<template>
  <div
      class="slider-wrapper"
      @touchstart="startTouch"
      @touchend="endTouch"
  >
    <div
        class="slides-container"
        :style="{ transform: `translateX(-${currentIndex * 100}%)` }"
    >
      <Slide1 @next-slide="goToNextSlide"  />
      <Slide2 @nextSlide="goToNextSlideFromSlide2" />
      <Slide3 />
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue';
import Slide1 from './Slide1.vue';
import Slide2 from './Slide2.vue';
import Slide3 from './Slide3.vue';

const currentIndex = ref(0);
let startX = 0;

function startTouch(event) {
  startX = event.touches[0].clientX;
}



function goToNextSlide() {
  currentIndex.value = 1; // индекс второго слайда
}

function goToNextSlideFromSlide2() {
  console.log("Переключение на Slide3"); // для проверки
  currentIndex.value = 2;
}


</script>

<style scoped>


.slider-wrapper {
  width: 100%;

  overflow: hidden;
  position: relative;
}

.slides-container {
  display: flex;
  width: 100%;

  transition: transform 0.3s ease;
}

.slides-container > * {
  flex: 0 0 100%;
  height: 100%;
}

.arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  z-index: 10;
  background: rgba(0,0,0,0.5);
  border: none;
  color: white;
  font-size: 2rem;
  padding: 0.5rem 1rem;
  cursor: pointer;
  display: none;
}

.arrow.left {
  left: 10px;
}

.arrow.right {
  right: 10px;
}



</style>
