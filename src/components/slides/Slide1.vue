<template>

    <div class="slide">

      <img src="../../assets/slide1-bg.png" class="slide-image" />

      <button class="center-button" @click="$emit('next-slide')">

        <img src="../../assets/button.png" alt="button" />
      </button>

    </div>


</template>

<style scoped>

  .center-button {
    position: absolute;
    bottom: 0%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: none;
    border: none;
    padding: 0;
    cursor: pointer;
  }

  .center-button img {
    display: block;
    max-width: 200px;
  }


</style>
