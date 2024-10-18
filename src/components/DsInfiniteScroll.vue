<template>
  <div class="infinite-scroll" @scroll="handleScroll" ref="scrollContainer">
    <slot :items="visibleItems"></slot>
    <div v-if="loading" class="loading">Loading...</div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue';

const props = defineProps({
  items: {
    type: Array,
    required: true
  },
  loadMore: {
    type: Function,
    required: true
  },
  threshold: {
    type: Number,
    default: 200
  }
});

const visibleItems = ref([]);
const loading = ref(false);
const scrollContainer = ref(null);

const loadMoreItems = async () => {
  if (loading.value) return;
  loading.value = true;
  await props.loadMore();
  loading.value = false;
};

const handleScroll = () => {
  const container = scrollContainer.value;
  if (!container) return;

  const { scrollTop, scrollHeight, clientHeight } = container;
  if (scrollHeight - scrollTop - clientHeight < props.threshold) {
    loadMoreItems();
  }
};

watch(() => props.items, (newItems) => {
  visibleItems.value = newItems;
}, { deep: true });

onMounted(() => {
  visibleItems.value = props.items;
});
</script>

<style scoped>
.infinite-scroll {
  height: 400px;
  overflow-y: auto;
}

.loading {
  text-align: center;
  padding: 10px;
}
</style>
