<template>
  <div class="w-100 rounded overflow-hidden shadow-lg mb-5">
    <div class="m-10 p-5 flex justify-between items-center">
      <p>{{ info.name }}</p>
      <div class="flex justify-between items-center">
        <div @click="deleteNodeSelected" class="mr-5">delete</div>
        <div @click="editNodeSelected">edit</div>
      </div>
    </div>
    <div class="m-10 p-5 flex flex-col justify-between items-start">
      <template
        :key="indexComment"
        v-for="(comment, indexComment) in info.comments"
      >
        <p>
          by {{ comment.by }}
          <DateFormat :dateTime="comment.date" />
        </p>
        <div class="flex justify-between items-center pb-5">
          <p>{{ comment.comment }}</p>
        </div>
      </template>
    </div>
  </div>
</template>

<script setup>
import DateFormat from "./DateFormat.vue";

// Define the custom event
const emit = defineEmits(["deleteNode", "editNode"]);

const props = defineProps({
  info: Object,
  keyIndex: Number,
});

const deleteNodeSelected = () => {
  // Emit the delete node event with the current count
  emit("deleteNode", { info: props.info });
};

const editNodeSelected = () => {
  // Emit the edit node event with the current count
  emit("editNode", { info: props.info });
};
</script>
