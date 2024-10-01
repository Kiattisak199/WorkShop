<template>
  <v-card class="mx-auto" max-width="344">
    <v-img height="200px" :src="product.Image" cover> </v-img>

    <v-card-title>{{ product.Name }}</v-card-title>
    <v-card-title>{{ product.Amount }}</v-card-title>
     
    <v-card-actions>
      <v-btn color="orange-lighten-2" text>Explore</v-btn>
      <v-spacer></v-spacer>
      <v-btn    
        :icon="show ? 'mdi-chevron-up' : 'mdi-chevron-down'"
        @click="show = !show"
      ></v-btn>     
    </v-card-actions>
<!-- Hi -->
    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>
        <v-card-text>{{ product.description }}</v-card-text>
      </div>
    </v-expand-transition>
  </v-card>
</template>

<script setup lang="ts">

const props = defineProps({
  product: {
    type: Object,
    required: true,
  },
});

const show = ref(false);
</script>
