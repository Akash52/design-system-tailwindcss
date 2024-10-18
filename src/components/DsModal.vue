<template>
  <div class="modal-documentation bg-gray-100 p-6 rounded-lg shadow-md  mx-auto my-8">
    <h2 class="text-2xl font-bold mb-6 text-gray-800">Modal Component Usage Guide</h2>
    
    <div class="space-y-6">
      <div>
        <h3 class="text-lg font-semibold mb-2 text-gray-700">Description</h3>
        <p class="text-gray-600">A reusable modal component for Vue 3 applications.</p>
      </div>
      
      <div>
        <h3 class="text-lg font-semibold mb-2 text-gray-700">Props</h3>
        <ul class="list-disc pl-5 text-gray-600">
          <li><strong>modelValue</strong> (Boolean): Controls the visibility of the modal. Use v-model for two-way binding.</li>
        </ul>
      </div>
      
      <div>
        <h3 class="text-lg font-semibold mb-2 text-gray-700">Emits</h3>
        <ul class="list-disc pl-5 text-gray-600">
          <li><strong>update:modelValue</strong>: Emitted when the modal should be closed.</li>
        </ul>
      </div>
      
      <div>
        <h3 class="text-lg font-semibold mb-2 text-gray-700">Slots</h3>
        <ul class="list-disc pl-5 text-gray-600">
          <li><strong>default</strong>: The main content of the modal.</li>
          <li><strong>title</strong>: The title of the modal.</li>
          <li><strong>footer</strong>: The footer content of the modal. By default, it contains a close button.</li>
        </ul>
      </div>
      
      <div>
        <h3 class="text-lg font-semibold mb-2 text-gray-700">Usage Example</h3>
        <pre class="bg-gray-800 text-green-400 p-4 rounded-md overflow-x-auto">
<code>&lt;modal v-model="showModal"&gt;
  &lt;template #title&gt;My Modal Title&lt;/template&gt;
  &lt;p&gt;This is the modal content.&lt;/p&gt;
  &lt;template #footer&gt;
    &lt;button @click="showModal = false"&gt;Custom Close&lt;/button&gt;
  &lt;/template&gt;
&lt;/modal&gt;</code>
        </pre>
      </div>
      
      <div>
        <h3 class="text-lg font-semibold mb-2 text-gray-700">Notes</h3>
        <ul class="list-disc pl-5 text-gray-600">
          <li>The modal uses teleport to render at the body level for better stacking context.</li>
          <li>It includes a transition for smooth enter/leave animations.</li>
          <li>The backdrop can be clicked to close the modal.</li>
          <li>The component uses Tailwind CSS classes for styling.</li>
        </ul>
      </div>
    </div>
  </div>

  <Teleport to="body">
    <Transition name="modal">
      <div v-if="modelValue" class="fixed inset-0 z-50 overflow-y-auto" aria-labelledby="modal-title" role="dialog" aria-modal="true">
        <div class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
          <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" aria-hidden="true" @click="$emit('update:modelValue', false)"></div>
          <span class="hidden sm:inline-block sm:align-middle sm:h-screen" aria-hidden="true">&#8203;</span>
          <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full">
            <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
              <div class="sm:flex sm:items-start">
                <div class="mt-3 text-center sm:mt-0 sm:ml-4 sm:text-left">
                  <h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-title">
                    <slot name="title">Modal Title</slot>
                  </h3>
                  <div class="mt-2">
                    <slot></slot>
                  </div>
                </div>
              </div>
            </div>
            <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
              <slot name="footer">
                <ds-button @click="$emit('update:modelValue', false)" variant="primary" class="w-full sm:ml-3 sm:w-auto">
                  Close
                </ds-button>
              </slot>
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
import DsButton from './DsButton.vue';

defineProps({
  modelValue: Boolean
});

defineEmits(['update:modelValue']);
</script>

<style scoped>
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}
</style>
