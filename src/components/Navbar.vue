<template>
  <div>
    <div
      v-if="searchIsSet === true"
      class="bg-black cursor-pointer w-screen h-screen fixed top-0 left-0 opacity-50 z-10"
      @click="setSearchState(false)"
    ></div>
    <nav class="bg-white py-4 fixed top-0 shadow-md w-full z-20">
      <div class="mx-5 md:mx-10 lg:mx-32 items-center flex justify-between">
        <figure class="w-4/12"></figure>
        <div v-if="searchIsSet === false" class="flex-grow flex justify-end">
          <button
            class="inline-flex items-center mr-5 py-2 px-3 rounded-md hover:border-yellow-300 border-gray-300 border"
            @click="setSearchState(true)"
          >
            <icon :icon="icons.search" /><span class="ml-2">Search</span>
          </button>
          <button
            class="inline-flex items-center py-2 px-3 rounded-md hover:border-yellow-300 border-gray-300 border"
            @click="openFilter"
          >
            <icon :icon="icons.filter" /><span class="ml-2">Filter</span>
          </button>
        </div>
        <div v-if="searchIsSet === true" class="flex-grow flex items-center">
          <form id="search" @submit.prevent="enterSearch"></form>
          <div class="w-7/12">
            <input
              v-model="searchData"
              type="text"
              class="w-full h-10 text-lg outline-none border-b border-gray-400"
              placeholder="Enter user's name"
              form="search"
            />
          </div>
          <div class="w-2/12 ml-3">
            <primary-btn
              type="submit"
              form="search"
              width="w-full"
              font-color="text-black"
            >
              <icon :icon="icons.search" />
            </primary-btn>
          </div>
          <div class="w-3/12 ml-3 text-gray-300 hover:text-gray-500">
            <button @click="setSearchState(false)">
              <icon width="36" height="36" :icon="icons.x" />
            </button>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
import { Icon } from '@iconify/vue';
import search from '@iconify/icons-feather/search';
import filter from '@iconify/icons-feather/filter';
import x from '@iconify/icons-feather/x';
import PrimaryBtn from './PrimaryBtn';

export default {
  name: 'Navbar',
  components: {
    Icon,
    PrimaryBtn,
  },
  data() {
    return {
      icons: {
        search,
        filter,
        x,
      },
      searchIsSet: false,
      searchData: '',
    };
  },
  mounted() {
    const v = this;
    window.addEventListener('keyup', (event) => {
      event.preventDefault();
      if (event.keyCode === 27) v.setSearchState(false);
    });
  },
  methods: {
    setSearchState(state) {
      this.searchIsSet = state;
    },
    enterSearch() {
      this.searchIsSet = false;
      this.$emit('enterSearch', this.searchData);
    },
    openFilter() {
      this.$emit('openFilter');
    },
  },
};
</script>

<style></style>
