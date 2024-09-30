<template>
  <div>
    <v-dialog v-model="internalDialog" persistent max-width="600px">
      <v-card>
        <v-card-title>
          <span class="text-h5">Create New Product</span>
        </v-card-title>

        <v-card-text>
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-text-field
              v-model="formData.Name"
              label="Product Name"
              :rules="[(v) => !!v || 'Name is required']"
              required
              variant="outlined"
              rounded
            ></v-text-field>

            <v-textarea
              v-model="formData.description"
              label="Product Description"
              :rules="[(v) => !!v || 'Description is required']"
              required
              variant="outlined"
              rounded
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
              variant="outlined"
              rounded
            ></v-text-field>

            <v-text-field
              v-model="formData.Image"
              label="Product Image URL"
              :rules="[(v) => !!v || 'Image URL is required']"
              required
              variant="outlined"
              rounded
            ></v-text-field>
          </v-form>
        </v-card-text>

        <v-card-actions>
          <v-spacer />
          <v-btn color="blue darken-1" @click="closeDialog" rounded>Cancel</v-btn>
          <v-btn
            color="blue darken-1"
            :disabled="!valid"
            @click="submitForm"
            rounded
          >Create</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script setup lang="ts">
const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false,
  },
});
const emit = defineEmits(["update:modelValue", "productCreated"]);

const valid = ref(false);
const formData = ref({
  Name: "",
  description: "",
  Amount: 0,
  Image: "",
});

const internalDialog = computed({
  get: () => props.modelValue,
  set: (value) => emit("update:modelValue", value),
});

// Close the dialog
const closeDialog = () => {
  internalDialog.value = false;
  resetForm();
};

// Emit the form data to the parent
const submitForm = () => {
  if (valid.value) {
    emit("productCreated", { ...formData.value });
    closeDialog();
  }
};

// Reset the form after submission or cancellation
const resetForm = () => {
  formData.value = {
    Name: "",
    description: "",
    Amount: 0,
    Image: "",
  };
};
</script>
