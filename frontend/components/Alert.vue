<template>
    <div v-if="visible" :class="['alert', alertType]" @click="closeAlert">
      <span class="material-icons">{{ icon }}</span>
      <span class="message">{{ message }}</span>
    </div>
  </template>
  
  <script setup lang="ts">
  
  // Define props interface
  interface Props {
    message: string;
    type?: 'success' | 'fail'; // Optional with specific string types
    duration?: number; // Optional
  }
  
  // Use defineProps to define props with types
  const props = defineProps<Props>();
  
  const visible = ref(true);
  const alertType = ref(props.type || 'success'); // Default to 'success' if type is not provided
  const icon = ref(props.type === 'success' ? 'check_circle' : 'error');
  
  const closeAlert = () => {
    visible.value = false;
  };
  
  // Auto-hide functionality
  setTimeout(() => {
    closeAlert();
  }, props.duration ?? 3000); // Default duration if not provided
  </script>
  
  <style scoped>
  .alert {
    display: flex;
    align-items: center;
    padding: 1rem;
    margin: 1rem 0;
    border-radius: 5px;
    color: #fff;
    cursor: pointer;
  }
  
  .alert.success {
    background-color: #4caf50; /* Green for success */
  }
  
  .alert.fail {
    background-color: #f44336; /* Red for fail */
  }
  
  .material-icons {
    margin-right: 0.5rem;
  }
  </style>
  