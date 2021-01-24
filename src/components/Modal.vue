<template>
  <div v-if="isopen === true" class="container">
    <transition-group tag="div">
      <aside
        key="backdrop"
        class="modal-backdrop"
        @click.prevent="closeModal"
      ></aside>
      <div key="modal" class="modal">
        <header v-if="deactivateCloseBtn == false">
          <button
            href=""
            class="text-sm py-1 px-2 rounded-md border border-gray-400 hover:border-yellow-400 flex items-center"
            @click.prevent="closeModal"
          >
            <icon :icon="chevronLeft" />
            <span class="inline-block ml-2">Back</span>
          </button>
        </header>
        <div class="body">
          <slot></slot>
        </div>
      </div>
    </transition-group>
  </div>
</template>

<script>
import { Icon } from '@iconify/vue';
import chevronLeft from '@iconify/icons-feather/chevron-left';

export default {
  components: {
    Icon,
  },
  props: {
    isopen: {
      type: Boolean,
      default: false,
    },
    deactivateCloseBtn: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      body: document.querySelector('body'),
      chevronLeft,
    };
  },
  watch: {
    isopen(newValue) {
      if (newValue === true) {
        return this.hideScrollBar();
      }
      return this.showScrollBar();
    },
  },
  mounted() {
    const v = this;
    window.addEventListener('keyup', (event) => {
      event.preventDefault();
      if (event.keyCode === 27) v.closeModal();
    });
  },
  methods: {
    closeModal() {
      if (this.deactivateCloseBtn === false) {
        return this.$emit('update:isopen', false);
      }
      return this;
    },
    hideScrollBar() {
      this.body.classList.add('overflow-y-hidden');
    },
    showScrollBar() {
      this.body.classList.remove('overflow-y-hidden');
    },
  },
};
</script>

<style scoped>
.modal-backdrop {
  background: rgba(0, 0, 0, 0.6);
  z-index: 1000;
  @apply w-screen h-screen block fixed top-0 left-0 right-0 bottom-0;
}
.modal {
  z-index: 1100;
  @apply w-screen h-screen bg-white fixed top-0 right-0 bottom-0;
}
.modal > header {
  height: 8vh;
  @apply border-b border-gray-300 px-4 py-2;
}
.modal > .body {
  @apply overflow-y-auto p-5;
  height: 92vh;
}
@screen md {
  .modal {
    @apply w-6/12;
  }
}
@screen lg {
  .modal {
    @apply w-3/12;
  }
}
</style>
