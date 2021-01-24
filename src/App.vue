<template>
  <div class="pt-24">
    <modal :isopen="modalOpen" @update:isopen="modalOpen = $event">
      <section v-if="userProfileIsOpen === true">
        <h2 class="text-2xl font-bold text-left">User Profile</h2>
      </section>
      <section v-if="filterIsOpen === true">
        <h2 class="text-2xl text-left font-bold mb-10">Filters</h2>
        <div class="mb-5">
          Filter By Payment Method
          <div class="mt-2 flex flex-wrap">
            <radio-input
              v-for="(paymentType, index) in paymentFilterList"
              :key="index"
              :value="paymentType.value"
              name="paymentFilter"
              :default-value="filters.paymentMethod"
              :title="paymentType.title"
              @input="changePaymentFilter($event)"
            />
          </div>
        </div>
        <div class="mb-10">
          Filter By Gender
          <div class="mt-2 flex flex-wrap">
            <radio-input
              v-for="(gender, index) in genderFilterList"
              :key="index"
              :value="gender"
              name="genderFilter"
              :default-value="filters.gender"
              :title="gender"
              @input="changeGenderFilter($event)"
            />
          </div>
        </div>
        <div>
          <primary-btn
            v-if="filters.paymentMethod !== null || filters.gender !== null"
            width="w-full"
            color="yellow"
            font-color="black"
            @clik="clearFilter"
            >Clear Filter</primary-btn
          >
        </div>
      </section>
    </modal>
    <navbar @open-filter="openFilter()" @enterSearch="enterSearch($event)" />
    <section class="mx-5 md:mx-10 lg:mx-32 mb-10">
      <user-list
        v-for="(person, index) in pagination.data"
        :key="index"
        :person="person"
        @openUserCard="openUserCard($event)"
      />
      <div class="mt-">
        <pagination
          :next-disabled="nextDisabled"
          :previous-disabled="previousDisabled"
          :current-page="pagination.currentPage"
          :last-page="pagination.lastPage"
          @enterNextPage="incrementPage"
          @enterPreviousPage="decrementPage"
        />
      </div>
    </section>
  </div>
</template>
<script>
// import { Icon } from '@iconify/vue';
import Navbar from './components/Navbar';
import Modal from './components/Modal';
import UserList from './components/UserListCard';
import Pagination from './components/Pagination';
import RadioInput from './components/RadioInput';
import PrimaryBtn from './components/PrimaryBtn';

export default {
  components: {
    Navbar,
    Modal,
    UserList,
    Pagination,
    RadioInput,
    PrimaryBtn,
  },
  data() {
    return {
      paymentFilterList: [
        {
          title: 'Cheque',
          value: 'check',
        },
        {
          title: 'Cash',
          value: 'cc',
        },
        {
          title: 'Paypal',
          value: 'paypal',
        },
        {
          title: 'Money Order',
          value: 'money order',
        },
      ],
      genderFilterList: ['Female', 'Male', 'Prefer To Skip'],
      searchInput: '',
      modalOpen: false,
      selectedPerson: {},
      persons: [],
      userProfileIsOpen: false,
      filterIsOpen: false,
      pagination: {
        start: 0,
        end: 19,
        interval: 20,
        currentPage: 1,
        firstPage: 1,
        lastPage: 1,
        data: [],
      },
      filters: {
        paymentMethod: null,
        gender: null,
      },
      cachedData: [],
    };
  },
  computed: {
    nextDisabled() {
      if (this.pagination.currentPage >= this.pagination.lastPage) {
        return true;
      }
      return false;
    },
    previousDisabled() {
      if (this.pagination.currentPage <= this.pagination.firstPage) {
        return true;
      }
      return false;
    },
  },
  mounted() {
    const vm = this;
    // 'https://api.enye.tech/v1/challenge/records'
    fetch('https://api.enye.tech/v1/challenge/records')
      .then((response) => response.json())
      .then((response) => {
        vm.cachedData = response.records ? response.records.profiles : [];
        vm.paginateData(vm.filterData());
      });
  },
  methods: {
    changePaymentFilter(value) {
      this.filters.paymentMethod = value;
      this.pagination.currentPage = 1;
      this.paginateData(this.filterData());
    },
    changeGenderFilter(value) {
      this.filters.gender = value;
      this.pagination.currentPage = 1;
      this.paginateData(this.filterData());
    },
    clearFilter() {
      this.filters.paymentMethod = null;
      this.filters.gender = null;
    },
    enterSearch(searchValue) {
      this.searchInput = searchValue;
      this.pagination.currentPage = 1;
      this.paginateData(this.filterData());
    },
    openFilter() {
      this.filterIsOpen = true;
      this.userProfileIsOpen = false;
      this.modalOpen = true;
    },
    openUserCard(person) {
      this.selectedPerson = person;
      this.filterIsOpen = false;
      this.userProfileIsOpen = true;
      this.modalOpen = true;
    },
    incrementPage() {
      window.scrollTo(0, 0);
      if (this.pagination.currentPage < this.pagination.lastPage) {
        const current = this.pagination.currentPage + 1;
        this.pagination.currentPage = current;
        this.paginateData(this.filterData());
      }
    },
    decrementPage() {
      window.scrollTo(0, 0);
      if (this.pagination.currentPage > this.pagination.firstPage) {
        const current = this.pagination.currentPage - 1;
        this.pagination.currentPage = current;
        this.paginateData(this.filterData());
      }
    },
    filterData() {
      if (
        this.searchInput === '' &&
        !this.filters.gender &&
        !this.filters.paymentMethod
      ) {
        return this.cachedData;
      }
      return this.cachedData.filter((person) => {
        return (
          this.searchIsPositive(person, this.searchInput) &&
          this.paymentMethodFilterIsPositive(
            person,
            this.filters.paymentMethod
          ) &&
          this.genderFilterIsPositive(person, this.filters.gender)
        );
      });
    },
    searchIsPositive(person = {}, searchInput = '') {
      if (searchInput === '' || person === {}) {
        return true;
      }
      return (
        person.FirstName.toLowerCase().includes(
          this.searchInput.toLowerCase()
        ) ||
        person.LastName.toLowerCase().includes(this.searchInput.toLowerCase())
      );
    },
    paymentMethodFilterIsPositive(person = {}, paymentFilter = null) {
      if (paymentFilter === null || person === {}) {
        return true;
      }
      return person.PaymentMethod === paymentFilter;
    },
    genderFilterIsPositive(person = {}, genderFilter = null) {
      if (genderFilter === null || person === {}) {
        return true;
      }
      return person.Gender === genderFilter;
    },
    paginateData(filteredData = []) {
      this.pagination.lastPage = Math.ceil(
        filteredData.length / this.pagination.interval
      );
      this.pagination.start =
        (this.pagination.currentPage - 1) * this.pagination.interval;
      this.pagination.end = this.pagination.start + this.pagination.interval;
      this.pagination.data = filteredData.slice(
        this.pagination.start,
        this.pagination.end
      );
    },
  },
};
</script>
<style>
html {
  scroll-behavior: smooth;
}
</style>
