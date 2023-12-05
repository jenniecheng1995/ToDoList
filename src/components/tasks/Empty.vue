<script setup>
import { defineProps } from "vue";

const props = defineProps({
  variant: {
    type: String,
    required: true,
    default: "tonal",
  },
  color: {
    type: String,
    required: true,
    default: "teal-lighten-4",
  },
});
</script>

<template>
  <v-card class="empty-card mx-auto mb-4 p-4" :variant="variant" :color="color">
    <v-card-item>
      <v-row>
        <v-col cols="12" align="center" justify="center">
          <img src="@/assets/empty_data.png" width="200" />
          <div class="text-h6" color>It's empty in here!</div>
        </v-col>
      </v-row>
    </v-card-item>
  </v-card>
</template>

<style scoped>
.empty-card {
  height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>
