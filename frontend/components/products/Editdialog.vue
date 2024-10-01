<template>
  <v-dialog
    v-model="internalDialog"
    :value="modelValue"
    persistent
    max-width="600px"
  >
    <v-card>
      <v-card-title>
        <span class="text-h5">Edit Product</span>
      </v-card-title>
      {{ fromData}}
      <v-card-text>
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-text-field
            v-model="fromData.Name"
            label="Product Name"
            :rules="[(v) => !!v || 'Name is required']"
            required
          ></v-text-field>

          <v-textarea
            v-model="fromData.description"
            label="Product Description"
            :rules="[(v) => !!v || 'Description is required']"
            required
          ></v-textarea>

          <v-text-field
            v-model="fromData.Amount"
            label="Quantity"
            type="number"
            :rules="[
              (v) => !!v || 'Quantity is required',
              (v) => v > 0 || 'Quantity must be positive',
            ]"
            required
          ></v-text-field>
          <v-text-field
            v-model="fromData.Image"
            label="Image"
            type="text"
            :rules="[(v) => !!v || 'Image is required']"
            required
          ></v-text-field>
        </v-form>
      </v-card-text>

      <v-card-actions>
        <v-spacer />
        <v-btn @click="closeDialog">Cancel</v-btn>
        <v-btn :disabled="!valid" @click="submitEditForm">Save</v-btn>
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
const fromData = ref({ Name: "", description: "", Amount: 0, Image: "" });

// watch(
//   () => props.product,
//   (newVal) => {
//     if (newVal) {
//       fromData.value = { ...newVal };
//     }
//   },
//   { immediate: true }
// );
watch(
  () => props.product,
  (newVal) => {
    if (newVal) {
      fromData.value = { ...newVal };
    } else {
      fromData.value = { Name: "", description: "", Amount: 0, Image: "" }; // Reset the form if no product is provided
    }
  },
  { immediate: true }
);
const internalDialog = computed({
  get: () => props.modelValue,
  set: (value) => emit("update:modelValue", value),
});

const closeDialog = () => {
  emit("update:modelValue", false);
};

// Submit edited product
// const submitEditForm = () => {
//   const updatedProduct = {
//     data: {
//       Name: fromData.value.Name,
//       Amount: fromData.value.Amount,
//       description: fromData.value.description,
//       Image: fromData.value.Image,
//     },
//   };
  
//   emit("updateProduct", updatedProduct);
//   closeDialog();
// };

const submitEditForm = () => {
  const updatedProduct = {
    id: props.product.id, // Include the ID of the product
    data: {
      Name: fromData.value.Name,
      Amount: fromData.value.Amount,
      description: fromData.value.description,
      Image: fromData.value.Image,
    },
  };

  emit("updateProduct", updatedProduct); // Emit updated product
  closeDialog();
};
</script>
