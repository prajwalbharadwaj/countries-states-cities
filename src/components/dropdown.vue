<script setup>
import { ref, computed, reactive, defineProps, defineModel, defineEmits } from 'vue';
import { onClickOutside } from '@vueuse/core'

const props = defineProps({
  options: { type: Array, default: () => [], },
  label: { type: String, default: '' },
});

const selected = defineModel('selected')
const emit = defineEmits(['update:selected'])
const target = ref(null)
const state = reactive({
  search: '',
  isOpen: false,
});

onClickOutside(target, event => state.isOpen = false)

const filteredOptions = computed(() => {
  return props.options.filter(item => item.name.toLowerCase().includes(state.search.toLowerCase()))
})
</script>

<template>
  <div>
    <div>{{ label || 'Select' }}</div>
    <div class="dropdown">
      <div class="dropdown-trigger" @click="state.isOpen = !state.isOpen">
        {{ selected?.name || label || 'Select' }}
      </div>
      <div v-if="state.isOpen" class="dropdown-content" ref="target">
        <input type="text" v-model="state.search" placeholder="Search..." />
        <hr />
        <div class="dropdown-content-list">
          <template v-if="filteredOptions.length">
            <div v-for="item in filteredOptions" :key="item.id" class="dropdown-content-item"
              @click="emit('update:selected', item); state.isOpen = false">
              {{ item.name }}
            </div>
          </template>
          <div v-else class="dropdown-no-result">No result</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.dropdown {
  position: relative;
  display: inline-block;
  font-size: 14px;
}

input {
  width: 100%;
  outline: none;
  border: 0;
  padding: 12px;
}

hr {
  display: block;
  height: 1px;
  border: 0;
  border-top: 1px solid #ccc;
}

.dropdown-trigger {
  border: 1px solid #ccc;
  padding: 4px 8px;
  border-radius: 4px;
  min-width: 250px;
  cursor: pointer;
}

.dropdown-trigger:hover {
  background: #f8f9fa;
}

.dropdown-content {
  position: absolute;
  z-index: 1;
  top: 100%;
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
  background: #fff;
  min-width: 250px;
}

.dropdown-content-list {
  max-height: 300px;
  overflow-y: auto;
}

.dropdown-content-item {
  padding: 8px;
  cursor: pointer;
}

.dropdown-content-item:hover {
  background: #e9ecef;
}

.dropdown-no-result {
  display: grid;
  justify-items: center;
  color: #495057;
  padding: 40px 0;
}
</style>