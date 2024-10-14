<template>
  <v-container>
    <div class="d-flex justify-space-between">
      <v-card-title class="text-h5 font-weight-bold">Product</v-card-title>
      <v-btn color="black" @click="dialog = true" class="mb-5 mt-2"
        >Create Product</v-btn
      >
    </div>

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
        <v-card class="pa-5" elevation="0">
          <div class="mb-5">
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
    
    <Alert
        v-if="alertMessage"
        :message="alertMessage"
        :type="alertType"
        :duration="5000"
      />
  </v-container>
</template>

<script setup lang="ts">
const products = ref([]);
const dialog = ref(false);
const editDialog = ref(false);
const deleteDialog = ref(false);
const selectedProduct = ref(null);

const alertMessage = ref<string>(''); // Define type for alertMessage
const alertType = ref<'success' | 'fail'>('success'); // Define type for alertType


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
    alertMessage.value = 'Product created successfully!';
    alertType.value = 'success';
    console.log("Product created successfully:", result);
  } catch (error) {
    console.error("Error creating product:", error);
  }
};
interface ProductUpdateData {
  Name: string;
  Amount: number;
  description?: string;
  Image?: string;
}

const updateProduct = async (updatedProduct: ProductUpdateData) => {
  console.log("Updating product:", selectedProduct.value);  // Debug log to check product
  if (!selectedProduct.value) return;

  const productId = selectedProduct.value.documentId;  // Assuming documentId is correct

  if (!productId) {
    console.error("Product ID is missing.");
    return;
  }

  try {
    const response = await fetch(`http://localhost:1337/api/products/${productId}`, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        data: {
          Name: updatedProduct.Name,         // Include updated fields
          Amount: updatedProduct.Amount,
          description: updatedProduct.description,
          Image: updatedProduct.Image,
        },
      }),
    });

    if (!response.ok) {
      const errorData = await response.json();
      console.error("Error from server:", errorData);
      throw new Error(`Failed to update product. Error: ${response.statusText}`);
    }

    const result = await response.json();
    alertMessage.value = 'Product updated successfully!';
    alertType.value = 'success';
    console.log("Product updated successfully:", result);
    fetchProducts()
  } catch (error) {
    console.error("Error updating product:", error);
  }
};
const deleteProduct = async () => {
  console.log("Deleting product:", selectedProduct.value);
  if (!selectedProduct.value) return;

  const productId = selectedProduct.value.documentId;

  if (!productId) {
    console.error("Product ID is missing.");
    return;
  }

  try {
    const response = await fetch(`http://localhost:1337/api/products/${productId}`, {
      method: "DELETE",
      headers: {
        "Content-Type": "application/json",
      },
    });

    if (!response.ok) {
      const errorData = await response.json();
      console.error("Error from server:", errorData);
      throw new Error(`Failed to delete product. Error: ${response.statusText}`);
    }

    console.log("Product deleted successfully.");
    // Remove product from local list after deletion
    products.value = products.value.filter((product) => product.documentId !== productId);
  } catch (error) {
    console.error("Error deleting product:", error);
  }
};

const openEditDialog = (product) => {
  selectedProduct.value = product;
  editDialog.value = true;
  console.log(product.documentId)
};
const openDeleteDialog = (product) => {
  selectedProduct.value = product;
  deleteDialog.value = true;
};

onMounted(() => {
  fetchProducts();
});
</script>
