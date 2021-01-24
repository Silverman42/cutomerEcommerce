<template>
  <div
    tabindex="-1"
    class="flex justify-between items-center bg-white rounded-lg p-3 md:p-5 mb-5 hover:border-yellow-100 text-left border-4 cursor-pointer shadow-none hover:shadow-md transition-all ease-in duration-200"
    @click="viewProfile(person)"
  >
    <div class="flex w-11/12 justify-between">
      <div class="w-8/12 lg:w-3/12 flex">
        <div class="mr-3">
          <figure class="w-10 h-10 rounded-full bg-yellow-200">
            <img
              class="object-cover object-center w-full"
              :src="`./img/${getUserAvatar(person.Gender)}`"
              alt="avatar"
            />
          </figure>
        </div>
        <div>
          <p class="text-sm font-semibold">
            {{ person.FirstName }} {{ person.LastName }}
          </p>
          <div class="flex text-gray-400">
            <span class="inline-block"><icon :icon="icons.mail" /></span>
            <a
              :href="`mailto:${person.Email}`"
              class="hover:text-yellow-300 lowercase inline-block ml-2 text-xs mt-0"
            >
              {{ person.Email }}</a
            >
          </div>
        </div>
      </div>
      <div class="w-3/12 hidden md:flex">
        <span class="inline-block mr-3 text-yellow-300">
          <icon width="30" height="30" :icon="icons.payment" />
        </span>
        <div>
          <p class="text-sm capitalize font-semibold">
            {{ person.CreditCardType }}
          </p>
          <span class="mt-0 text-xs block text-gray-400">Credit Card Type</span>
        </div>
      </div>
      <div class="w-3/12 hidden md:flex">
        <span class="inline-block mr-3 text-yellow-300">
          <icon width="30" height="30" :icon="icons.location" />
        </span>
        <div>
          <p class="text-sm capitalize font-semibold">
            {{ person.Latitude }}<sup>o</sup>La. / {{ person.Longitude
            }}<sup>o</sup>Lo.
          </p>
          <span class="mt-0 text-xs block text-gray-400">Location</span>
        </div>
      </div>
    </div>
    <span class="inline-block text-gray-500">
      <icon :icon="icons.chevronRight" />
    </span>
  </div>
</template>

<script>
import { Icon } from '@iconify/vue';
import payment from '@iconify/icons-feather/credit-card';
import location from '@iconify/icons-feather/map-pin';
import mail from '@iconify/icons-feather/mail';
import chevronRight from '@iconify/icons-feather/chevron-right';

export default {
  components: {
    Icon,
  },
  props: {
    person: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  data() {
    return {
      icons: {
        payment,
        location,
        mail,
        chevronRight,
      },
    };
  },
  methods: {
    viewProfile(person) {
      return this.$emit('openUserCard', person);
    },
    getUserAvatar(gender) {
      if (gender === 'Male') return 'man.svg';
      if (gender === 'Female') return 'woman.svg';
      return 'unknown.svg';
    },
  },
};
</script>

<style></style>
