<template>
  <transition name="fade">
    <aside v-if="isopen === true" class="modal-backdrop" @click="closeModal" />
  </transition>
</template>

<script>
export default {
  props: {
    isopen: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    closeModal() {
      this.$emit('update:isopen', false);
    },
  },
};
</script>

<style>
.modal-backdrop {
  background: rgba(0, 0, 0, 0.6);
  z-index: 1000;
  @apply w-screen h-screen block fixed top-0 left-0 right-0 bottom-0 cursor-pointer;
}
.fade-enter-active {
  animation: fade 1s;
  -webkit-animation: fade 1s;
}

.fade-leave-active {
  animation: fade 0.2s reverse;
  -webkit-animation: fade 0.3s reverse;
}
@keyframes fade {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}
</style>
