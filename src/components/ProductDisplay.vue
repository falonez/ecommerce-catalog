<template>
  <div class="split-background">
    <!-- Bagian atas -->
    <div :class="['top-section', { 'man-background': isManBackground }]">
      <CardComponent @isMan="sendDataToParent" />
    </div>
  </div>
</template>

<script>
import CardComponent from './CardComponent.vue'
export default {
  data() {
    return {
      isManBackground: false
    }
  },
  methods: {
    changeBackgroundColor() {
      this.isManBackground = !this.isManBackground
    },
    sendDataToParent(isMan) {
      this.isManBackground = isMan
    }
  },
  components: {
    CardComponent
  }
}
</script>

<style>
.split-background {
  height: 70vh;
  position: relative;
}

.top-section {
  position: absolute;
  background-color: var(--color-background-women);
  width: 100%;
  height: 100%;
  background-image: url(@/assets/img/bg-pattern.svg);
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}

.man-background {
  background-color: var(--color-background-man);
}
</style>
