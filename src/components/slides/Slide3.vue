<template>
  <div class="slide">
    <img src="../../assets/slide3-bg.png" class="slide-image" />


    <img
        v-if="showBubble4"
        src="../../assets/Bubble4.png"
        class="bubble bubble1"
        :class="{ pulse: !bubble4Clicked }"
        @click="onBubble4Click"
        alt="Bubble 4"
    />


    <img
        v-if="showBubble5"
        src="../../assets/Bubble5.png"
        class="bubble bubble2"
        :class="{ pulse: isBubble5Playing }"
        alt="Bubble 5"
    />
  </div>
</template>

<script setup>
import { ref, onBeforeUnmount } from "vue";
import bubble4AudioUrl from "../../assets/bubble4.m4a";
import bubble5AudioUrl from "../../assets/bubble5.m4a";

const showBubble4 = ref(true);
const bubble4Clicked = ref(false);

const showBubble5 = ref(false);
const isBubble5Playing = ref(false);

const audio4 = new Audio(bubble4AudioUrl);
const audio5 = new Audio(bubble5AudioUrl);

let bubble4Played = false;
let bubble5Played = false;

function onBubble4Click() {
  if (bubble4Played) return;
  bubble4Played = true;
  bubble4Clicked.value = true;


  showBubble4.value = false;


  audio4.onended = () => {
    showBubble5.value = true;
    playBubble5();
  };

  audio4.currentTime = 0;
  audio4.play().catch(() => {
    // Если play() не сработал — всё равно показать Bubble5 через короткую задержку
    setTimeout(() => {
      showBubble5.value = true;
      playBubble5();
    }, 300);
  });
}

function playBubble5() {
  if (bubble5Played) return;
  bubble5Played = true;
  isBubble5Playing.value = true;

  audio5.onended = () => {
    isBubble5Playing.value = false;
    showBubble5.value = false;
  };

  audio5.currentTime = 0;
  audio5.play().catch(() => {
    setTimeout(() => {
      isBubble5Playing.value = false;
      showBubble5.value = false;
    }, 600);
  });
}

onBeforeUnmount(() => {
  audio4.pause();
  audio4.src = "";
  audio5.pause();
  audio5.src = "";
});
</script>


<style scoped>

.bubble {
  position: absolute;
  width: 15%;
  cursor: pointer;
  user-select: none;
}

.bubble1 {
  top: 14%;
  left: 14%;
}

.bubble2 {
  top: 21%;
  left: 60%;
  transform: translateX(-50%);
}

.pulse {
  animation: pulse 1s infinite;
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.15); }
}
</style>
