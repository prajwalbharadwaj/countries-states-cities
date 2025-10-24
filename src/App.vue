<script setup>
import { reactive, computed, watch } from 'vue';
import data from '@/assets/csc';
import Dropdown from '@/components/Dropdown.vue';
import CodeMirrorEditor from '@/components/CodeMirrorEditor.vue';
import Footer from '@/components/Footer.vue';

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
      class="container-width" label="Select Country" />
    <Dropdown v-if="state.selectedCountry?.id && allStates?.length" :key="state.selectedCountry?.id"
      v-model:selected="state.selectedState" :options="allStates" placeholder="Select a state" class="container-width"
      label="Select State" />
    <Dropdown v-if="state.selectedState?.id && allCities?.length" v-model:selected="state.selectedCities"
      :options="allCities" placeholder="Select a City" class="container-width" label="Select City" />

  </div>
  <div class="container">
    <div class="container-width" v-if="state.selectedCountry?.id">
      Selected Country:
      <CodeMirrorEditor :jsonCode="JSON.stringify(state.selectedCountry, null, 2)" />
    </div>

    <div class="container-width" v-if="state.selectedState?.id">
      Selected State:
      <CodeMirrorEditor :jsonCode="JSON.stringify(state.selectedState, null, 2)" />
    </div>

    <div class="container-width" v-if="state.selectedCities?.id">
      Selected Cities:
      <CodeMirrorEditor :jsonCode="JSON.stringify(state.selectedCities, null, 2)" />
    </div>
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

.container-width {
  @media screen and (min-width: 768px) {
    width: 32%;
  }

  @media screen and (max-width: 768px) {
    width: 100%;
  }
}
</style>
