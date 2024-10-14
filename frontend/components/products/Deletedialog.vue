<template>
  <!-- Use :value instead of v-model and @update:modelValue to handle changes -->
  <v-dialog
    v-model="internalDialog"
    :value="modelValue"
    max-width="400px"
    @update:modelValue="onDialogClose"
  >
    <v-card  rounded="xl" elevation="0">
      <v-card-title class="ma-2 text-h5 font-weight-bold text-center">Confirm Delete</v-card-title>
      <v-card-text class="my-n3 ml-1 font-weight-regular text-h7">Are you sure you want to delete this product?</v-card-text>

      <v-card-actions class="mb-2">
        <v-spacer />
        <v-btn rounded @click="cancelDelete">Cancel</v-btn>
        <v-btn rounded class="px-3" color="red darken-1" variant="outlined"  @click="confirmDelete">Delete</v-btn>
        <v-spacer />
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup lang="ts">
const props = defineProps({
  modelValue: Boolean,
});


const internalDialog = computed(() => props.modelValue);
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
