<template>
  <v-container>
    <v-card-title class="font-weight-bold">Product</v-card-title>
    <v-btn color="primary" @click="dialog = true" class="mb-5"
      >Create Product</v-btn
    >  

    <ProductsCreatedialog
      v-model="dialog"
      @productCreated="handleProductCreated"
    />
    <v-row>
      <v-col
        v-for="product in products"
        :key="product.id"
        cols="12"
        sm="6"
        md="4"
      >
        <v-card class="pa-5">
          <div class="mb-3">
            <ProductsListMenu
              @edit="openEditDialog(product)" 
              @delete="openDeleteDialog(product)"
            />
          </div>
          <ProductsCard :product="product" />
        </v-card>
      </v-col>
    </v-row>
    <ProductsEditdialog
      v-model="editDialog"
      :product="selectedProduct"
      @update:modelValue="editDialog = $event"
      @updateProduct="updateProduct"
    />
    <ProductsDeletedialog
      v-model="deleteDialog"
      @update:modelValue="deleteDialog = $event"
      @confirmDelete="deleteProduct"
    />
  </v-container>
</template>
<!-- ้่เิ่า้เ่า้เ -->
<script setup lang="ts">
const products = ref([]);
const dialog = ref(false);
const editDialog = ref(false);
const deleteDialog = ref(false);
const selectedProduct = ref(null);

const fromData = ref({
  Name: "",
  Amount: "",
  description: "",
  Image: "",
  // Add other fields if necessary
});

// Fetch products from the API
const fetchProducts = async () => {
  try {
    const response = await fetch("http://localhost:1337/api/products");
    const data = await response.json();
    products.value = data.data;
  } catch (error) {
    console.error("Error fetching products:", error);
  }
};
//Create Product
const handleProductCreated = async (newProduct) => {
  try {
    const response = await fetch("http://localhost:1337/api/products", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ data: newProduct }),
    });

    const result = await response.json();
    products.value.push(result.data);
    console.log("Product created successfully:", result);
  } catch (error) {
    console.error("Error creating product:", error);
  }
};
const updateProduct = async (updatedProduct) => {
  if (!selectedProduct.value) return;

  const productId = selectedProduct.value.id;
  try {
    const response = await fetch(`http://localhost:1337/api/products/${productId}`, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ data: updatedProduct }),
    });

    if (!response.ok) {
      throw new Error(`Error: ${response.statusText}`);
    }

    const result = await response.json();
    
    // Update the product in the products list
    const index = products.value.findIndex((p) => p.id === productId);
    if (index !== -1) {
      products.value[index] = result.data;
    }

    editDialog.value = false; // Close the edit dialog
    console.log("Product updated successfully:", result);
  } catch (error) {
    console.error("Error updating product:", error);
  }
};

const openEditDialog = (product) => {
  selectedProduct.value = product;
  editDialog.value = true;
};
const openDeleteDialog = (product) => {
  selectedProduct.value = product;
  deleteDialog.value = true;
};

const addNewProduct = (newProduct) => {
  products.value.push(newProduct);
};

onMounted(() => {
  fetchProducts();
});
</script>
