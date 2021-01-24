<template>
  <div>
    <div
      class="flex justify-between rounded-xl bg-gradient-to-tr to-gray-600 from-gray-900 py-5 p-3 md:p-5 mb-8"
    >
      <div class="flex-grow mr-2">
        <h5 class="text-yellow-300 text-lg">
          {{ profileData.FirstName || '' }} {{ profileData.LastName || '' }}
        </h5>
        <a
          class="lowercase inline-block mt-1 text-xs text-gray-200 rounded-lg px-3 py-1 border border-gray-600"
          :href="`mailto:${profileData.Email || ''}`"
          >{{ profileData.Email || '' }}</a
        >
      </div>
      <figure class="w-16 h-16 rounded-full bg-yellow-200">
        <img
          class="object-center object-cover w-full"
          :src="`./img/${getProfileImageName}`"
          alt="avatar"
          srcset=""
        />
      </figure>
    </div>
    <div class="details">
      <h6>username</h6>
      <p>{{ profileData.UserName || '' }}</p>
    </div>
    <div class="details">
      <h6>gender</h6>
      <p>{{ profileData.Gender || '' }}</p>
    </div>
    <div class="details">
      <h6>phone number</h6>
      <p>{{ profileData.PhoneNumber || '' }}</p>
    </div>
    <div class="details">
      <h6>last login</h6>
      <p>{{ profileData.LastLogin || '' }}</p>
    </div>
    <div class="details">
      <h6>location</h6>
      <p>
        {{ `${profileData.Longitude || ''}` }} <sup>o</sup> Longitude /
        {{ `${profileData.Latitude || ''}` }} <sup>o</sup> Latitude
      </p>
    </div>
    <div class="details">
      <h6>credit card type</h6>
      <p>{{ `${profileData.CreditCardType || ''}` }}</p>
    </div>
    <div class="details">
      <h6>credit card number</h6>
      <p>{{ `${profileData.CreditCardNumber || ''}` }}</p>
    </div>
    <div class="details">
      <h6>payment method</h6>
      <p>{{ profileData.PaymentMethod || '' }}</p>
    </div>
    <div class="details">
      <h6>domain name</h6>
      <p>{{ profileData.DomainName || '' }}</p>
    </div>
    <div class="details">
      <h6><span class="uppercase">URL</span> Address</h6>
      <a :href="`http://${profileData.URL || ''}`"
        >{{ profileData.URL || '' }}
      </a>
    </div>
    <div class="details">
      <h6><span class="uppercase">Mac</span> Address</h6>
      <p>{{ `${profileData.CreditCardNumber || ''}` }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    profileData: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  computed: {
    getProfileImageName() {
      if (this.profileData.Gender === 'Male') return 'man.svg';
      if (this.profileData.Gender === 'Female') return 'woman.svg';
      return 'unknown.svg';
    },
  },
};
</script>

<style scoped>
.details {
  @apply mb-7;
}

.details > h6 {
  @apply text-xs text-gray-500 capitalize;
}

.details > p {
  @apply text-lg capitalize;
}
.details > a {
  @apply text-lg capitalize hover:text-yellow-400;
}
</style>
