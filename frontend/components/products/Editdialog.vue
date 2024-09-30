<template>
  <v-dialog
    v-model="internalDialog"
    :value="modelValue"
    persistent
    max-width="600px"
    @input="closeDialog"
  >
    <v-card>
      <v-card-title>
        <span class="text-h5">Edit Product</span>
      </v-card-title>
      {{ formData }}
      <v-card-text>
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-text-field
            v-model="formData.Name"
            label="Product Name"
            :rules="[(v) => !!v || 'Name is required']"
            required
          ></v-text-field>

          <v-textarea
            v-model="formData.description"
            label="Product Description"
            :rules="[(v) => !!v || 'Description is required']"
            required
          ></v-textarea>

          <v-text-field
            v-model="formData.Amount"
            label="Quantity"
            type="number"
            :rules="[
              (v) => !!v || 'Quantity is required',
              (v) => v > 0 || 'Quantity must be positive',
            ]"
            required
          ></v-text-field>
          <v-text-field
            v-model="formData.Image"
            label="Image"
            type="text"
            :rules="[(v) => !!v || 'Image is required']"
            required
          ></v-text-field>
        </v-form>
      </v-card-text>

      <v-card-actions>
        <v-spacer />
        <v-btn text @click="closeDialog">Cancel</v-btn>
        <v-btn :disabled="!valid" text @click="submitEditForm">Save</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup lang="ts">
const props = defineProps({
  modelValue: Boolean,
  product: Object,
});
const emit = defineEmits(["update:modelValue", "updateProduct"]);
const valid = ref(false);
const fromData =ref({ Name: '', description: '', Amount: 0, Image: '' });

watch(
  () => props.product,
  (newVal) => {
    if (newVal) {
        fromData.value = { ...newVal }; // Copy the product data
    }
  },
  { immediate: true }
);
const internalDialog = computed({
  get: () => props.modelValue,
});

const closeDialog = () => {
  emit("update:modelValue", false);
};

// Submit edited product
const submitEditForm = () => {
  const updatedProduct = { ...props.product, attributes: formData.value };
  emit("updateProduct", updatedProduct);
  closeDialog();
};
</script>
