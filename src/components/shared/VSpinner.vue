<script setup></script>
<template>
  <div class="spinner">
    <span class="spinner-inner-1"></span>
    <span class="spinner-inner-2"></span>
    <span class="spinner-inner-3"></span>
  </div>
</template>

<style lang='scss' scoped>
@import '../../assets/styles/main';

.spinner {
  width: $size;
  height: $size;
  animation: spinner 0.75s linear infinite;

  span {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 50%;
  }
  
  .spinner-inner-1 {
    background: linear-gradient(to right, rgba($spinner,0) 0%,rgba($spinner,0) 50%,$spinner 51%);
  }
  
  .spinner-inner-2 {
    background: linear-gradient(to top, rgba($main-bg-color,0) 0%,$main-bg-color 100%);
  }
  
  .spinner-inner-3 {
    $offset: $size * 0.1;
    top: $offset;
    left: $offset;
    width: $size - ($offset * 2);
    height: $size - ($offset * 2);
    background: $main-bg-color;
  }
}

@keyframes spinner {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>