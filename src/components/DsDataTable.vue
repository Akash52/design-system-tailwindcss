<template>
  <div class="overflow-x-auto">
    <table class="min-w-full divide-y divide-gray-200">
      <thead class="bg-gray-50">
        <tr>
          <th v-for="column in columns" :key="column.key" scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
            {{ column.label }}
            <span v-if="column.sortable" class="ml-1 cursor-pointer" @click="sort(column.key)">
              {{ sortKey === column.key ? (sortOrder === 'asc' ? '▲' : '▼') : '▲▼' }}
            </span>
          </th>
        </tr>
      </thead>
      <tbody class="bg-white divide-y divide-gray-200">
        <tr v-for="item in sortedData" :key="item.id">
          <td v-for="column in columns" :key="column.key" class="px-6 py-4 whitespace-nowrap">
            <slot :name="column.key" :item="item">
              {{ item[column.key] }}
            </slot>
          </td>
        </tr>
      </tbody>
    </table>
    <div v-if="pagination" class="bg-white px-4 py-3 flex items-center justify-between border-t border-gray-200 sm:px-6">
      <div class="flex-1 flex justify-between sm:hidden">
        <ds-button :disabled="currentPage === 1" @click="changePage(currentPage - 1)" variant="secondary">Previous</ds-button>
        <ds-button :disabled="currentPage === totalPages" @click="changePage(currentPage + 1)" variant="secondary">Next</ds-button>
      </div>
      <div class="hidden sm:flex-1 sm:flex sm:items-center sm:justify-between">
        <div>
          <p class="text-sm text-gray-700">
            Showing
            <span class="font-medium">{{ startIndex + 1 }}</span>
            to
            <span class="font-medium">{{ endIndex }}</span>
            of
            <span class="font-medium">{{ totalItems }}</span>
            results
          </p>
        </div>
        <div>
          <nav class="relative z-0 inline-flex rounded-md shadow-sm -space-x-px" aria-label="Pagination">
            <ds-button
              v-for="page in visiblePages"
              :key="page"
              @click="changePage(page)"
              :variant="page === currentPage ? 'primary' : 'secondary'"
              class="relative inline-flex items-center px-4 py-2 border text-sm font-medium"
            >
              {{ page }}
            </ds-button>
          </nav>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import DsButton from './DsButton.vue';

const props = defineProps({
  columns: {
    type: Array,
    required: true
  },
  data: {
    type: Array,
    required: true
  },
  pagination: {
    type: Object,
    default: null
  }
});

const sortKey = ref('');
const sortOrder = ref('asc');
const currentPage = ref(1);

const sort = (key) => {
  if (sortKey.value === key) {
    sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc';
  } else {
    sortKey.value = key;
    sortOrder.value = 'asc';
  }
};

const sortedData = computed(() => {
  let result = [...props.data];
  if (sortKey.value) {
    result.sort((a, b) => {
      let modifier = sortOrder.value === 'asc' ? 1 : -1;
      if (a[sortKey.value] < b[sortKey.value]) return -1 * modifier;
      if (a[sortKey.value] > b[sortKey.value]) return 1 * modifier;
      return 0;
    });
  }
  if (props.pagination) {
    const start = (currentPage.value - 1) * props.pagination.itemsPerPage;
    return result.slice(start, start + props.pagination.itemsPerPage);
  }
  return result;
});

const totalItems = computed(() => props.data.length);
const totalPages = computed(() => Math.ceil(totalItems.value / props.pagination.itemsPerPage));
const startIndex = computed(() => (currentPage.value - 1) * props.pagination.itemsPerPage);
const endIndex = computed(() => Math.min(startIndex.value + props.pagination.itemsPerPage, totalItems.value));

const visiblePages = computed(() => {
  const delta = 2;
  const range = [];
  const rangeWithDots = [];
  let l;

  for (let i = 1; i <= totalPages.value; i++) {
    if (i === 1 || i === totalPages.value || (i >= currentPage.value - delta && i <= currentPage.value + delta)) {
      range.push(i);
    }
  }

  range.forEach(i => {
    if (l) {
      if (i - l === 2) {
        rangeWithDots.push(l + 1);
      } else if (i - l !== 1) {
        rangeWithDots.push('...');
      }
    }
    rangeWithDots.push(i);
    l = i;
  });

  return rangeWithDots;
});

const changePage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page;
  }
};
</script>
