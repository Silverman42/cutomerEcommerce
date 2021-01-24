<template>
  <button
    class="inline-flex rounded-md h-12 px-5 items-center justify-center transition-all duration-300 ease-in-out"
    :class="[
      buttonPrimaryStyling.hover,
      buttonPrimaryStyling.primary,
      width,
      extraClass,
      buttonStateCursor,
      fontColor,
    ]"
    :disabled="disabled == true || loading == true"
    :type="type"
    @click="trigger($event)"
  >
    <span v-if="loading == true" class="loader">
      <i></i>
    </span>
    <span v-else>
      <slot></slot>
    </span>
  </button>
</template>

<script>
export default {
  name: 'PrimaryBtn',
  props: {
    color: {
      type: String,
      default: 'yellow',
    },
    fontColor: {
      type: String,
      default: 'text-white',
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
    extraClass: {
      type: String,
      default: '',
    },
  },
  computed: {
    buttonPrimaryStyling() {
      if (this.disabled === true || this.loading === true) {
        return {
          hover: `bg-${this.color}-200`,
          primary: ``,
        };
      }
      return {
        hover: `bg-${this.color}-400`,
        primary: `hover:bg-${this.color}-600`,
      };
    },
    buttonStateCursor() {
      if (this.disabled === true) return 'cursor-not-allowed';
      if (this.loading === true) return 'cursor-wait';
      return '';
    },
  },
  methods: {
    trigger(event) {
      this.$emit('clik', event);
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
