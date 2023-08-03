<script setup>
import defineEmits from 'vue'

const emit = defineEmits('changeView');

const emitEvent = () => emit('changeView')
</script>


<template>
  <button class="primary-button" @click="emitEvent">
    <slot></slot>
  </button>
</template>


<style lang='scss' scoped>
.primary-button {
  background-color: transparent;
  border: 0;
  cursor: pointer;
  font-size: 18px;
}
</style>