<template>
  <div class="border rounded-md">
    <div v-for="(item, index) in items" :key="index" class="border-b last:border-b-0">
      <button
        @click="toggleItem(index)"
        class="flex justify-between items-center w-full p-4 text-left"
        :aria-expanded="openItems[index]"
        :aria-controls="`accordion-content-${index}`"
      >
        <span class="font-medium">{{ item.title }}</span>
        <svg
          class="w-5 h-5 transition-transform duration-200"
          :class="{ 'transform rotate-180': openItems[index] }"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
        </svg>
      </button>
      <div
        v-show="openItems[index]"
        :id="`accordion-content-${index}`"
        class="p-4 bg-gray-50"
      >
        {{ item.content }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  items: {
    type: Array,
    required: true
  },
  multiple: {
    type: Boolean,
    default: false
  }
});

const openItems = ref(props.items.map(() => false));

const toggleItem = (index) => {
  if (props.multiple) {
    openItems.value[index] = !openItems.value[index];
  } else {
    openItems.value = openItems.value.map((_, i) => i === index ? !openItems.value[i] : false);
  }
};
</script>
