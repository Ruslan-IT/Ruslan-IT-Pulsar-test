<template>
  <div class="slide">
    <img src="../../assets/slide2-bg.png" class="slide-image" />


    <button
        class="sound-button"
        :class="{ pulse: !bubblesStarted }"
        @click="startBubbles"
    >
      <img src="../../assets/button_volume.png" alt="sound" />
    </button>


    <img
        v-if="bubblesStarted && currentBubble === 1"
        src="../../assets/Bubble1.png"
        class="bubble bubble1"
        :class="{ pulse: !allClicked }"
        @click="nextBubble"
        alt="Bubble 1"
    />

    <img
        v-if="bubblesStarted && currentBubble === 2"
        src="../../assets/Bubble2.png"
        class="bubble bubble2"
        :class="{ pulse: !allClicked }"
        @click="nextBubble"
        alt="Bubble 2"
    />

    <img
        v-if="bubblesStarted && currentBubble === 3"
        src="../../assets/Bubble3.png"
        class="bubble bubble3"
        :class="{ pulse: !allClicked }"
        @click="nextBubble"
        alt="Bubble 3"
    />
  </div>
</template>

<script setup>
import { ref } from "vue";

let bubble3Played = false;

const emit = defineEmits(["nextSlide"]);

import bubble1Audio from "../../assets/bubble1.m4a";
import bubble2Audio from "../../assets/bubble2.m4a";
import bubble3Audio from "../../assets/bubble3.m4a";

const bubblesStarted = ref(false);
const currentBubble = ref(1);
const allClicked = ref(false);
const maxBubbles = 3;




const bubbleSounds = [
  new Audio(bubble1Audio),
  new Audio(bubble2Audio),
  new Audio(bubble3Audio)
];

function playSound(index, onEnd = null) {
  const audio = bubbleSounds[index];
  audio.currentTime = 0;

  if (onEnd) {
    audio.onended = () => {
      audio.onended = null;
      onEnd();
    };
  }

  audio.play().catch(() => {});
}

function startBubbles() {
  if (bubblesStarted.value) return;

  bubblesStarted.value = true;
  console.log("bubblesStarted = true")
  // Запускаем Bubble1 звук
  playSound(0);
}


function nextBubble() {
  if (currentBubble.value < maxBubbles) {
    currentBubble.value++;

    // проверка для Bubble3
    if (currentBubble.value === 3) {
      if (!bubble3Played) {
        bubble3Played = true;
        playSound(2); // Bubble3
      }
    } else {
      playSound(currentBubble.value - 1); // Bubble1, Bubble2
    }

  } else if (!allClicked.value) {
    allClicked.value = true;
    emit("nextSlide");
  }
}
</script>

<style scoped>

.sound-button {
  position: absolute;
  top: 5%;
  right: 5%;
  width: 100px;
  background: none;
  border: none;
  cursor: pointer;
}

.sound-button img {
  width: 100%;
}


.bubble {
  position: absolute;
  width: 15%;
  cursor: pointer;
  transition: transform 0.2s;
}


.bubble1 {
  top: 20%;
  left: 20%;
}

.bubble2 {
  top: 20%;
  left: 50%;
  transform: translateX(-50%);
}

.bubble3 {
  top: 24%;
  left: 70%;
}

.pulse {
  animation: pulse 1s infinite;
}

@keyframes pulse {
  0%,100% { transform: scale(1); }
  50% { transform: scale(1.15); }
}
</style>
