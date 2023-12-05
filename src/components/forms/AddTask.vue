<script setup>
import { ref, defineProps, defineEmits } from "vue";

const alertMsg = "Please type something in the input field.";
const themeColor = "teal-lighten-5";
const initTask = {
  content: "",
  level: 1,
  isFinish: false,
};

const showAlert = ref(false);
const task = ref({
  content: "",
  level: 1,
  isFinish: false,
});

defineProps({
  variant: {
    type: String,
    required: true,
    default: "elevated",
  },
  color: {
    type: String,
    required: true,
    default: "teal",
  },
});

const emits = defineEmits(["add-task"]);

const addTask = () => {
  if (task.value.content == "") {
    showAlert.value = true;
    return;
  }
  emits("add-task", task.value);
  task.value = Object.assign({ ...initTask });
};

const clearTask = () => {
  task.value = Object.assign({ ...initTask });
};
</script>

<template>
  <v-card class="mx-auto mb-4" :color="color" :variant="variant">
      <v-container>
        <v-row>
          <v-col cols="12">
            <v-rating
              class="mb-3"
              v-model="task.level"
              :length="3"
              :size="24"
              :color="themeColor"
              :active-color="themeColor"
              size="x-small"
            />
            <v-text-field
              v-model.trim="task.content"
              variant="filled"
              clear-icon="mdi-close-circle"
              clearable
              label="New Daily Task"
              type="text"
              @keydown.enter="addTask"
              @click:clear="clearTask"
            ></v-text-field>
            <v-btn
              class="me-1 text-none"
              color="teal-lighten-5"
              prepend-icon="mdi-plus"
              variant="tonal"
              @click="addTask"
            >
              Add
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
  </v-card>
  <v-snackbar v-model="showAlert" :timeout="2000" color="teal-lighten-3" variant="tonal">
    {{ alertMsg }}
  </v-snackbar>
</template>
