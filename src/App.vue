<script setup>
import { reactive, computed, watch } from 'vue';
import data from '@/assets/csc';
import Dropdown from '@/components/dropdown.vue';
import Footer from '@/components/footer.vue';

const state = reactive({
  selectedCountry: null,
  selectedState: null,
  selectedCities: null,
});

const allCountries = computed(() => data);
const allStates = computed(() => state?.selectedCountry?.states || []);
const allCities = computed(() => state?.selectedState?.cities || []);

watch(() => state.selectedCountry?.id, () => {
  state.selectedState = null;
  state.selectedCities = null;
})
</script>

<template>
  <div class="container">
    <Dropdown v-model:selected="state.selectedCountry" :options="allCountries" placeholder="Select a country"
      class="w-full md:w-56" label="Select Country" />
    <Dropdown v-if="state.selectedCountry?.id && allStates?.length" :key="state.selectedCountry?.id"
      v-model:selected="state.selectedState" :options="allStates" placeholder="Select a state" class="w-full md:w-56"
      label="Select State" />
    <Dropdown v-if="state.selectedState?.id && allCities?.length" v-model:selected="state.selectedCities"
      :options="allCities" placeholder="Select a City" class="w-full md:w-56" label="Select City" />
  </div>
  <Footer class="footer-container" />
</template>

<style scoped>
.container {
  padding: 8px;
  gap: 12px;

  @media screen and (min-width: 768px) {
    display: flex;
  }

  @media screen and (max-width: 768px) {
    display: grid;
  }
}

.footer-container {
  position: fixed;
  bottom: 0;
}
</style>
