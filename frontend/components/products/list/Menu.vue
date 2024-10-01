<template>
  <!-- Speed Dial component -->
  <v-speed-dial
    v-model="fab"
    location="bottom center"
  >
    <template v-slot:activator="{ props: activatorProps }">
      <v-fab
        v-bind="activatorProps"
        color="primary"
        icon="mdi-dots-vertical"
      ></v-fab>
    </template>

    <!-- Action buttons for Edit and Delete -->
     <div class="mt-n3 ml-10">
       <div class="mt-9">
         <v-btn key="1" icon="mdi-pencil" @click="emitEdit" color="info"></v-btn>
       </div>
       <div class="mt-2">
         <v-btn
           key="2"
           icon="mdi-delete"
           @click="emitDelete"
           color="error"
         ></v-btn>
       </div>
     </div>
  </v-speed-dial>
</template>

<script setup lang="ts">
const fab = ref(false);
const emit = defineEmits(["edit", "delete"]);

const emitEdit = () => {
  emit("edit");
};
const emitDelete = () => {
  emit("delete");
};
</script>
