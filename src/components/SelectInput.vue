<template>
  <div>
    <label class="block text-xs mb-1 flex justify-between" :for="id">
      <p>{{ label }}</p>
      <div class="inline-block w-auto">
        <slot name="detail"></slot>
      </div>
    </label>
    <div
      class="flex mb-1 bg-primaryBg-200 border border-primaryBg-400 text-black h-12 overflow-hidden rounded leading-tight focus:bg-primaryBg-300 focus:border-primary-300"
      tabindex="-1"
    >
      <select
        v-bind="$attrs"
        :id="id"
        ref="select"
        v-model="selected"
        class="block appearance-none bg-primaryBg-200 pl-3 flex-grow focus:outline-none"
        :disabled="disabled == true || loading == true"
      >
        <slot></slot>
      </select>
      <div
        class="pointer-events-none inset-y-0 right-0 flex items-center px-1 text-gray-700"
      >
        <svg
          class="fill-current h-4 w-4"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 20 20"
        >
          <path
            d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
          />
        </svg>
      </div>
    </div>
    <div v-if="loading == true" class="loading-bar">
      <span></span>
    </div>
    <p v-if="error !== null" class="text-red-500 text-xs italic">{{ error }}</p>
  </div>
</template>

<script>
export default {
  name: 'SelectInput',
  inheritAttrs: false,
  props: {
    label: String,
    error: {
      type: String,
      default: null,
    },
    value: [String, Number, Boolean],
    disabled: {
      type: Boolean,
      default: false,
    },
    loading: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      id: `id${Math.floor(Math.random() * 50000000)}`,
      selected: this.value,
    };
  },
  watch: {
    selected(selected) {
      this.$emit('input', selected);
    },
  },
  methods: {
    input(event) {
      return this.$emit('input', event);
    },
    focus() {
      this.$refs.select.focus();
    },
    select() {
      this.$refs.select.select();
    },
    setSelectionRange(start, end) {
      this.$refs.select.setSelectionRange(start, end);
    },
  },
};
</script>

<style scoped>
.loading-bar {
  @apply h-1 rounded-md bg-primaryBg-400 overflow-hidden;
}
.loading-bar span {
  width: 10%;
  animation: slide 0.7s ease-in-out infinite;
  @apply h-2 bg-primary-400 block rounded-md;
}
@keyframes slide {
  from {
    transform: translateX(0%);
  }
  to {
    transform: translateX(1000%);
  }
}
</style>
