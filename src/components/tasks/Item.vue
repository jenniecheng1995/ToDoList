<script setup>
import { ref, defineProps, defineEmits, watch } from "vue";

const props = defineProps({
  variant: {
    type: String,
    required: true,
    default: "tonal",
  },
  color: {
    type: String,
    required: true,
    default: "teal-lighten-3",
  },
  task: {
    type: Object,
    required: true,
  },
});

const taskContentStyle = ref("");
const emits = defineEmits(["remove-book"]);

const removeTask = (e) => {
  const btn = e.currentTarget;
  const id = btn?.dataset.taskid;
  if (id) {
    emits("remove-task", id);
  }
};

watch(
  () => props.task?.isFinish,
  (newValue) => {
    if (newValue !== undefined) {
      taskContentStyle.value = newValue ? "finish-status" : "";
    }
  }
);
</script>

<template>
  <v-card class="mx-auto mb-4" :color="color" :variant="variant">
    <v-card-item>
      <v-row>
        <v-col cols="12">
          <div class="text-overline mb-1">
            <v-icon
              v-for="n in task.level"
              :key="n"
              icon="mdi-star"
              size="large"
            ></v-icon>
            <v-icon
              v-for="n in 3 - task.level"
              :key="n"
              icon="mdi-star"
              size="large"
              color="teal-lighten-5"
            ></v-icon>
          </div>
        </v-col>
      </v-row>
      <v-row align="center" justify="center">
        <v-col cols="2">
          <v-checkbox
            color="teal-lighten-3"
            v-model="task.isFinish"
            hide-details
          ></v-checkbox>
        </v-col>
        <v-col cols="8">
          <div class="text-h6" :class="taskContentStyle">{{ task.content }}</div>
        </v-col>
        <v-col cols="2">
          <v-btn
            variant="text"
            icon="mdi-trash-can"
            color="teal-lighten-4"
            :data-taskid="task.id"
            @click="removeTask"
          ></v-btn>
        </v-col>
      </v-row>
    </v-card-item>
  </v-card>
</template>

<style scoped>
.finish-status {
  text-decoration: line-through;
}
</style>
