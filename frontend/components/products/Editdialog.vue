<template>
  <v-dialog
    v-model="internalDialog"
    :value="modelValue"
    persistent
    max-width="600px"
  >
    <v-card rounded="xl" elevation="0">
      <v-card-title class="ma-2 mb-n3 text-h5 font-weight-medium text-start"
        >Edit Product
      </v-card-title>
      <v-card-text>
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-text-field
            v-model="fromData.Name"
            label="Product Name"
            :rules="[(v) => !!v || 'Name is required']"
            required
            variant="outlined"
            rounded
          ></v-text-field>

          <v-text-field
            v-model="fromData.Amount"
            label="Quantity"
            type="number"
            :rules="[
              (v) => !!v || 'Quantity is required',
              (v) => v > 0 || 'Quantity must be positive',
            ]"
            required
            variant="outlined"
            rounded
          ></v-text-field>
          <v-text-field
            v-model="fromData.Image"
            label="Image"
            type="text"
            :rules="[(v) => !!v || 'Image is required']"
            required
            variant="outlined"
            rounded
          ></v-text-field>
          <v-textarea
            v-model="fromData.description"
            label="Product Description"
            :rules="[(v) => !!v || 'Description is required']"
            required
            variant="outlined"
            rounded
          ></v-textarea>
        </v-form>
      </v-card-text>

      <v-card-actions class="mt-n8 mb-1">
        <v-spacer />
        <v-btn rounded @click="closeDialog">Cancel</v-btn>
        <v-btn
        rounded
        variant="flat"
        color="black"
        class="px-5"
          :disabled="!valid"
          @click="submitEditForm"
          >Save</v-btn
        >
        <v-spacer />
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
const fromData = ref({
  documentId: "",
  Name: "",
  description: "",
  Amount: 0,
  Image: "",
});

watch(
  () => props.product,
  (newVal) => {
    if (newVal) {
      // Assuming props.product has fields like `Name`, `description`, etc. directly
      fromData.value = {
        documentId: newVal.documentId || "",
        Name: newVal.Name || "",
        description: newVal.description || "",
        Amount: newVal.Amount || 0,
        Image: newVal.Image || "",
      };
    } else {
      // Reset to default when there's no product
      fromData.value = {
        documentId: "",
        Name: "",
        description: "",
        Amount: 0,
        Image: "",
      };
    }
  },
  { immediate: true }
);

const internalDialog = computed(() => props.modelValue);

const closeDialog = () => {
  emit("update:modelValue", false);
};

// Submit edited product
const submitEditForm = () => {
  const updatedProduct = { ...props.product, ...fromData.value }; // Flatten the product object
  emit("updateProduct", updatedProduct);
  closeDialog();
};
</script>
