<template>
  <button
    :class="[
      'px-4 py-2 rounded-md transition-colors',
      variant === 'primary' ? 'bg-blue-500 text-white hover:bg-blue-600' :
      variant === 'secondary' ? 'bg-gray-200 text-gray-800 hover:bg-gray-300' :
      'bg-white border border-gray-300 hover:bg-gray-100'
    ]"
  >
    <slot></slot>
  </button>
</template>

<script setup>
defineProps({
  variant: {
    type: String,
    default: 'default'
  }
});
</script>
