<template>
  <div>
    <h1>Question</h1>
    <button>Yes</button>
    <button
      ref="movingButton"
      @mousemove="moveButton"
      @click="onClick"
      :style="{ top: buttonY + 'px', left: buttonX + 'px' }"
    >
      No
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      buttonX: 0,
      buttonY: 0
    };
  },
  methods: {
    moveButton(event) {
      const button = this.$refs.movingButton;
      const buttonRect = button.getBoundingClientRect();
      const mouseX = event.clientX;
      const mouseY = event.clientY;
      const distance = 200; // Adjust this value as needed
      const viewportWidth = window.innerWidth;
      const viewportHeight = window.innerHeight;

      // Calculate new button position
      let newButtonX = buttonRect.left + buttonRect.width / 2;
      let newButtonY = buttonRect.top + buttonRect.height / 2;

      // Calculate direction of movement
      const deltaX = mouseX - newButtonX;
      const deltaY = mouseY - newButtonY;

      // Adjust button position based on direction of movement
      if (Math.abs(deltaX) > Math.abs(deltaY)) {
        newButtonX += deltaX > 0 ? distance : -distance;
        newButtonY += deltaY > 0 ? (deltaY / Math.abs(deltaX)) * distance : -(deltaY / Math.abs(deltaX)) * distance;
      } else {
        newButtonY += deltaY > 0 ? distance : -distance;
        newButtonX += deltaX > 0 ? (deltaX / Math.abs(deltaY)) * distance : -(deltaX / Math.abs(deltaY)) * distance;
      }

      // Ensure button stays within viewport
      newButtonX = Math.max(0, Math.min(viewportWidth - buttonRect.width, newButtonX - buttonRect.width / 2));
      newButtonY = Math.max(0, Math.min(viewportHeight - buttonRect.height, newButtonY - buttonRect.height / 2));

      // Update button position
      this.buttonX = newButtonX - buttonRect.width / 2;
      this.buttonY = newButtonY - buttonRect.height / 2;
    },
    onClick() {
      alert("Button clicked!");
    }
  }
};
</script>

<style scoped>
button {
  position: absolute;
}
</style>
