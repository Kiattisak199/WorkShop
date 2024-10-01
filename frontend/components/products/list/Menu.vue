<template>
  <!-- Speed Dial component -->
  <v-speed-dial   
    v-model="fab"
    location="bottom right"
    transition="fade-transition"
  >
    <template v-slot:activator="{ props: activatorProps }">
      <v-fab
        v-bind="activatorProps"
        color="primary"
        icon="mdi-dots-vertical"
      ></v-fab>
    </template>

    <!-- Action buttons for Edit and Delete -->
    <v-btn key="1" icon="mdi-pencil" @click="emitEdit" color="info"></v-btn>
    <v-btn key="2" icon="mdi-delete" @click="emitDelete" color="error"></v-btn>
  </v-speed-dial>
</template>

<script setup lang="ts">
const fab = ref(false);
const emit = defineEmits(['edit', 'delete']);

const emitEdit = () => {
  emit('edit');
};
const emitDelete = () => {
  emit('delete');
};
</script>
