<template>
  <!-- Use :value instead of v-model and @update:modelValue to handle changes -->
  <v-dialog
    v-model="internalDialog"
    :value="modelValue"
    max-width="400px"
    @update:modelValue="onDialogClose"
  >    
    <v-card>
      <v-card-title>Confirm Delete</v-card-title>
      <v-card-text>Are you sure you want to delete this product?</v-card-text>

      <v-card-actions>
        <v-spacer />
        <v-btn text @click="cancelDelete">Cancel</v-btn>
        <v-btn color="red darken-1" text @click="confirmDelete">Delete</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup lang="ts">
const props = defineProps({
  modelValue: Boolean,
});

const internalDialog = computed({
  get: () => props.modelValue
});
const emit = defineEmits(["update:modelValue", "confirmDelete"]);

// Close dialog handler
const onDialogClose = (value: boolean) => {
  emit("update:modelValue", value);
};

const cancelDelete = () => {
  emit("update:modelValue", false);
};

const confirmDelete = () => {
  emit("confirmDelete");
  emit("update:modelValue", false);
};
</script>
