<template>
  <div class="inline-flex w-full md:w-auto md:mr-7 mb-4 md:mb-0">
    <div class="mr-2 break-words flex-grow">
      <h6 class="text-xs capitalize text-gray-400">{{ heading }}</h6>
      <p class="capitalize">{{ body }}</p>
    </div>
    <button
      class="border border-gray-400 hover:border-gray-700 hover:text-gray-700 text-gray-400 rounded-full flex items-center justify-center w-5 h-5"
      @click="triggerFilterRemove"
    >
      <icon :icon="icons.x" />
    </button>
  </div>
</template>

<script>
import { Icon } from '@iconify/vue';
import x from '@iconify/icons-feather/x';

export default {
  components: {
    Icon,
  },
  props: {
    body: String,
    heading: String,
  },
  data() {
    return {
      icons: {
        x,
      },
    };
  },
  methods: {
    triggerFilterRemove() {
      this.$emit('removeFilter');
    },
  },
};
</script>

<style></style>
