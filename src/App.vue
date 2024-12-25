<script setup>
import { reactive, computed } from 'vue';
import data from '@/assets/csc';
import Dropdown from '@/components/dropdown.vue';

const state = reactive({
  selectedCountry: null,
  selectedState: null,
  selectedCities: null,
});

const allCountries = computed(() => data);
const allStates = computed(() => state?.selectedCountry?.states || []);
const allCities = computed(() => state?.selectedState?.cities || []);
</script>

<template>
  <div class="">
    <Dropdown v-model="state.selectedCountry" :options="allCountries" placeholder="Select a country"
      class="w-full md:w-56" />
    <Dropdown v-if="state.selectedCountry?.id" v-model="state.selectedState" :options="allStates"
      placeholder="Select a state" class="w-full md:w-56" />
    <Dropdown v-if="state.selectedState?.id" v-model="state.selectedCities" :options="allCities"
      placeholder="Select a City" class="w-full md:w-56" />
  </div>
</template>

<style scoped></style>
