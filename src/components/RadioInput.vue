<template>
  <label :for="id" class="container">
    <input
      :id="id"
      v-model="checked"
      type="radio"
      :name="name"
      :value="value"
      class="radio"
      @change="getInputValue($event)"
    />
    <div class="background">
      <span class="tick"></span>
      <p class="label capitalize">
        {{ title }}
      </p>
    </div>
  </label>
</template>

<script>
export default {
  name: 'RadioInput',
  props: {
    title: String,
    defaultValue: {
      default: '',
      validator: (prop) => typeof prop === 'string' || prop === null,
    },
    value: String,
    name: String,
  },
  data() {
    return {
      id: `id${Math.floor(Math.random() * 50000000)}`,
      checked: this.defaultValue,
    };
  },
  watch: {
    defaultValue(newValue) {
      this.checked = newValue;
    },
  },
  methods: {
    getInputValue(event) {
      return this.$emit('makeInput', event.target.value);
    },
  },
};
</script>

<style scoped>
.container {
  @apply inline-block w-auto mr-2 cursor-pointer mb-2;
}
.container:last-child {
  @apply mr-0;
}
.container > .radio {
  @apply hidden h-0 w-0;
}
.background {
  @apply inline-flex rounded-md h-12 w-auto px-4 items-center overflow-hidden bg-gray-300;
  @apply transition-all duration-300 ease-in-out;
}
.radio:checked ~ .background {
  @apply bg-gray-800;
}
.tick {
  @apply w-3 h-3 border-2 rounded-full inline-block mr-2 border-gray-600;
  @apply transition-all duration-300 ease-in-out;
}
.radio:checked ~ .background > .tick {
  @apply border-white bg-white;
}
.label {
  @apply transition-all duration-300 ease-in-out;
}
.radio:checked ~ .background > .label {
  @apply text-white;
}
</style>
