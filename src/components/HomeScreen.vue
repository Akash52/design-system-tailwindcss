<script setup>
import { ref } from 'vue'
import DsButton from './DsButton.vue';
import DsModal from './DsModal.vue';
import DsDataTable from './DsDataTable.vue';
import DsAccordion from './DsAccordion.vue';
import DsTabs from './DsTabs.vue';
import DsMultiStepForm from './DsMultiStepForm.vue';
import DsInfiniteScroll from './DsInfiniteScroll.vue';

const showModal = ref(false);

// Table columns
const columns = [
  { key: 'id', label: 'ID', sortable: true },
  { key: 'name', label: 'Name', sortable: true },
  { key: 'email', label: 'Email', sortable: true },
  { key: 'status', label: 'Status' }
];

// Table data
const tableData = [
  { id: 1, name: 'John Doe', email: 'john@example.com', status: 'Active' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com', status: 'Inactive' },
  { id: 3, name: 'Bob Johnson', email: 'bob@example.com', status: 'Active' },
  { id: 4, name: 'Alice Brown', email: 'alice@example.com', status: 'Active' },
  { id: 5, name: 'Charlie Davis', email: 'charlie@example.com', status: 'Inactive' },
  { id: 6, name: 'Eva Wilson', email: 'eva@example.com', status: 'Active' },
  { id: 7, name: 'Frank Miller', email: 'frank@example.com', status: 'Active' },
  { id: 8, name: 'Grace Lee', email: 'grace@example.com', status: 'Active' },
  { id: 9, name: 'Henry Clark', email: 'henry@example.com', status: 'Inactive' },
  { id: 10, name: 'Isabella Turner', email: 'isabella@example.com', status: 'Active' }
];

// Accordion items
const accordionItems = [
  { title: 'Section 1', content: 'Content for section 1' },
  { title: 'Section 2', content: 'Content for section 2' },
  { title: 'Section 3', content: 'Content for section 3' },
];

// Tab items
const tabItems = [
  { label: 'Tab 1', component: { template: '<div>Content for Tab 1</div>' } },
  { label: 'Tab 2', component: { template: '<div>Content for Tab 2</div>' } },
  { label: 'Tab 3', component: { template: '<div>Content for Tab 3</div>' } },
];

// Multi-step form steps
const formSteps = [
  { title: 'Step 1', component: { template: '<div>Step 1 content</div>' } },
  { title: 'Step 2', component: { template: '<div>Step 2 content</div>' } },
  { title: 'Step 3', component: { template: '<div>Step 3 content</div>' } },
];

// Infinite scroll items
const scrollItems = ref([...Array(20)].map((_, i) => ({ id: i, content: `Item ${i + 1}` })));

const loadMoreItems = () => {
  const newItems = [...Array(10)].map((_, i) => ({
    id: scrollItems.value.length + i,
    content: `Item ${scrollItems.value.length + i + 1}`
  }));
  scrollItems.value.push(...newItems);
};
</script>

<template>
 <div class="container mx-auto p-4">
    <h1 class="text-6xl font-extrabold bg-gradient-to-bl from-blue-500 to-blue-800 bg-clip-text text-transparent py-8">
      Design System Components 
    </h1>
    
    <h2 class="text-2xl font-extrabold bg-gradient-to-bl from-blue-500 to-blue-800 bg-clip-text text-transparent py-4">Modal</h2>
    <ds-button @click="showModal = true" variant="primary">Open Modal</ds-button>
    
    <ds-modal v-model="showModal">
      <template #title>Important Information</template>
      <p>This is an example of a reusable modal component. It can be used for various purposes such as confirmations, forms, or displaying detailed information.</p>
    </ds-modal>
    
    <h2 class="text-2xl font-extrabold bg-gradient-to-bl from-blue-500 to-blue-800 bg-clip-text text-transparent py-4">Data Table</h2>
    <ds-data-table
      :columns="columns"
      :data="tableData"
      :pagination="{ itemsPerPage: 5 }"
    >
      <template #status="{ item }">
        <span :class="{ 'text-green-600': item.status === 'Active', 'text-red-600': item.status === 'Inactive' }">
          {{ item.status }}
        </span>
      </template>
    </ds-data-table>

    <h2 class="text-2xl font-extrabold bg-gradient-to-bl from-blue-500 to-blue-800 bg-clip-text text-transparent py-4">Accordion</h2>
    <ds-accordion :items="accordionItems" class="mb-8" />

    <h2 class="text-2xl font-extrabold bg-gradient-to-bl from-blue-500 to-blue-800 bg-clip-text text-transparent py-4">Tabs</h2>
    <ds-tabs :tabs="tabItems" class="mb-8" />

    <h2 class="text-2xl font-extrabold bg-gradient-to-bl from-blue-500 to-blue-800 bg-clip-text text-transparent py-4">Multi-Step Form</h2>
    <ds-multi-step-form :steps="formSteps" class="mb-8" />

    <h2 class="text-2xl font-extrabold bg-gradient-to-bl from-blue-500 to-blue-800 bg-clip-text text-transparent py-4">Infinite Scroll</h2>
    <ds-infinite-scroll :items="scrollItems" :load-more="loadMoreItems">
      <template #default="{ items }">
        <div v-for="item in items" :key="item.id" class="p-4 border-b">
          {{ item.content }}
        </div>
      </template>
    </ds-infinite-scroll>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
