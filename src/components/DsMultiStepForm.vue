<template>
  <div>
    <div class="mb-4">
      <ol class="flex items-center w-full p-3 space-x-2 text-sm font-medium text-center text-gray-500 bg-white border border-gray-200 rounded-lg shadow-sm dark:text-gray-400 sm:text-base sm:p-4 sm:space-x-4">
        <li v-for="(step, index) in steps" :key="index" class="flex items-center" :class="{'text-blue-600 dark:text-blue-500': currentStep >= index}">
          <span class="flex items-center justify-center w-5 h-5 mr-2 text-xs border border-blue-600 rounded-full shrink-0 dark:border-blue-500" :class="{'bg-blue-600 text-white': currentStep > index}">
            {{ index + 1 }}
          </span>
          {{ step.title }}
          <svg v-if="index < steps.length - 1" aria-hidden="true" class="w-4 h-4 ml-2 sm:ml-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 5l7 7-7 7M5 5l7 7-7 7"></path></svg>
        </li>
      </ol>
    </div>

    <component :is="steps[currentStep].component" @next="nextStep" @prev="prevStep" />

    <div class="mt-4 flex justify-between">
      <ds-button @click="prevStep" :disabled="currentStep === 0" variant="secondary">Previous</ds-button>
      <ds-button @click="nextStep" :disabled="currentStep === steps.length - 1">
        {{ currentStep === steps.length - 1 ? 'Submit' : 'Next' }}
      </ds-button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import DsButton from './DsButton.vue';

const props = defineProps({
  steps: {
    type: Array,
    required: true
  }
});

const currentStep = ref(0);

const nextStep = () => {
  if (currentStep.value < props.steps.length - 1) {
    currentStep.value++;
  }
};

const prevStep = () => {
  if (currentStep.value > 0) {
    currentStep.value--;
  }
};
</script>
