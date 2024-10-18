<template>
  <div>
    <div class="border-b border-gray-200">
      <nav class="-mb-px flex" aria-label="Tabs">
        <button
          v-for="(tab, index) in tabs"
          :key="index"
          @click="activeTab = index"
          :class="[
            activeTab === index
              ? 'border-indigo-500 text-indigo-600'
              : 'border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300',
            'w-1/4 py-4 px-1 text-center border-b-2 font-medium text-sm'
          ]"
          :aria-current="activeTab === index ? 'page' : undefined"
        >
          {{ tab.label }}
        </button>
      </nav>
    </div>
    <div class="mt-4">
      <component :is="tabs[activeTab].component" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  tabs: {
    type: Array,
    required: true
  }
});

const activeTab = ref(0);
</script>
