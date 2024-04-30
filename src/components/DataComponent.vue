<template>
  <div class="w-100 rounded overflow-hidden shadow-lg mb-5">
    <div class="m-10 p-5 flex justify-between items-center">
      <p>{{ info.name }}</p>
      <div class="flex justify-between items-center">
        <div @click="deleteNodeSelected" class="mr-5">delete</div>
        <div @click="editNodeSelected">edit</div>
      </div>
    </div>
  </div>
</template>

<script setup>
// Define the custom event
const emit = defineEmits(['deleteNode', 'editNode']);


const props = defineProps({
    info: Object,
    keyIndex: Number,
})

const deleteNodeSelected = () => {
    // Emit the delete node event with the current count
    emit('deleteNode', {info: props.info});
}

const editNodeSelected = () => {
    // Emit the edit node event with the current count
    emit('editNode',  {info: props.info});
}
</script>
