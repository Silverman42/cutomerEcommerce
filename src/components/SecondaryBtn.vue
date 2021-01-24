<template>
  <button
    class="inline-flex rounded-md h-12 px-5 items-center justify-center font-bold transition-all duration-300 ease-in-out"
    :class="[
      `hover:bg-${color}-200`,
      `bg-${color}-100`,
      `text-${color}-700`,
      width,
      extraClass,
      disabled == true ? 'cursor-not-allowed' : '',
      loading == true ? 'cursor-wait' : '',
    ]"
    :disabled="disabled == true || loading == true"
    :type="type"
    @click="trigger($event)"
  >
    <span v-if="loading == true" class="loader">
      <i></i>
    </span>
    <div v-else class="flex items-center">
      <span class="inline-block"><slot name="icon"></slot></span>
      <span class="inline-block"><slot></slot></span>
    </div>
  </button>
</template>

<script>
export default {
  name: 'SecondaryBtn',
  props: {
    color: {
      type: String,
      default: 'primary',
    },
    width: {
      type: String,
      default: 'w-auto',
    },
    type: {
      type: String,
      default: 'button',
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    loading: {
      type: Boolean,
      default: false,
    },
    extraClass: String,
  },
  methods: {
    trigger(event) {
      this.$emit('click', event);
    },
  },
};
</script>
<style scoped>
.loader {
  @apply block w-24;
}
.loader > i {
  animation: loader 0.7s linear infinite;
  transform-origin: center;
  @apply block w-6 h-6 rounded-full border-2 border-white mx-auto;
  border-top-color: transparent;
  border-bottom-color: transparent;
}
@keyframes loader {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
