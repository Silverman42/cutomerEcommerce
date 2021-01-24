<template>
  <div class="load-bar_container">
    <figure class="load-avatar"></figure>
    <div class="w-8/12 md:w-3/12 inline-block load-bar">
      <span class="w-10/12 mb-2"></span>
      <span class="w-6/12"></span>
    </div>
    <div class="w-3/12 hidden md:inline-block load-bar">
      <span class="w-10/12 mb-2"></span>
      <span class="w-6/12"></span>
    </div>
    <div class="w-3/12 hidden md:inline-block load-bar">
      <span class="w-10/12 mb-2"></span>
      <span class="w-6/12"></span>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.load-bar > span {
  animation: glow 1s ease-in 0.1s infinite;
  @apply p-2 block rounded-xl bg-gray-200 mr-3;
}

.load-bar_container {
  @apply flex justify-between items-center bg-white rounded-lg p-3 md:p-5 mb-5 text-left border-4 cursor-pointer shadow-none;
}

.load-avatar {
  animation: glow 1s ease-in 0.1s infinite;
  @apply w-10 h-10 rounded-full bg-gray-300 mr-3;
}

@keyframes glow {
  0% {
    @apply bg-gray-200;
  }
  50% {
    @apply bg-gray-300;
  }
  100% {
    @apply bg-gray-200;
  }
}
</style>
