<template>
  <div
    class="border flex border-gray-300 rounded-md items-center p-2 w-8/12 md:w-6/12 lg:w-3/12 justify-between mx-auto"
  >
    <primary-btn
      :disabled="previousDisabled"
      color="yellow"
      font-color="text-black"
      @clik="enterPreviousPage"
    >
      <icon :icon="icons.arrowLeft" />
    </primary-btn>
    <div class="px-3 text-center">
      {{ currentPage }} / <sub>{{ lastPage }}</sub>
    </div>
    <primary-btn
      :disabled="nextDisabled"
      color="yellow"
      font-color="text-black"
      @clik="enterNextPage"
    >
      <icon :icon="icons.arrowRight" />
    </primary-btn>
  </div>
</template>

<script>
import { Icon } from '@iconify/vue';
import arrowRight from '@iconify/icons-feather/arrow-right';
import arrowLeft from '@iconify/icons-feather/arrow-left';
import PrimaryBtn from './PrimaryBtn';

export default {
  components: {
    PrimaryBtn,
    Icon,
  },
  props: {
    currentPage: {
      type: [Number, String],
      default: 1,
    },
    lastPage: {
      type: [Number, String],
      default: 1,
    },
    nextDisabled: {
      type: Boolean,
      default: false,
    },
    previousDisabled: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      icons: {
        arrowRight,
        arrowLeft,
      },
    };
  },
  methods: {
    enterNextPage() {
      this.$emit('enterNextPage');
    },
    enterPreviousPage() {
      this.$emit('enterPreviousPage');
    },
  },
};
</script>

<style></style>
