<script setup>
import { reactive, defineProps } from 'vue';

const props = defineProps({
  options: {
    type: Array,
    default: () => [],
  },
});

const state = reactive({
  isOpen: false,
});
</script>

<template>
  <div class="dropdown">
    <div class="dropdown-trigger" @click="state.isOpen = !state.isOpen">
      Dropdown
    </div>
    <div v-if="state.isOpen" class="dropdown-content">
      <div v-for="item in options" :key="item.id" class="dropdown-content-item">
        {{ item.name }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-content {
  /* display: none; */
  position: absolute;
  top: 100%;
  max-height: 300px;
  overflow-y: auto;
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
  /* left: 0;
  background-color: #fff;
  z-index: 1; */
}

.dropdown-content-item {
  padding: 8px;
}
</style>