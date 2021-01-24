<template>
  <button
    class="inline-flex rounded-md h-12 px-5 items-center justify-center transition-all duration-300 ease-in-out"
    :class="[
      buttonPrimaryStyling.hover,
      buttonPrimaryStyling.primary,
      width,
      extraClass,
      disabled == true ? 'cursor-not-allowed' : '',
      loading == true ? 'cursor-wait' : '',
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
      default: 'primary',
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
        hover: `bg-${this.color}-500`,
        primary: `hover:bg-${this.color}-700`,
      };
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
