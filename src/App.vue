<template>
  <div class="container mx-auto px-4">
    <h1 class="font-semibold m-10 p-5">List of Information</h1>
    <div v-if="!showCreateFrom">
      <button
        @click="showForm"
        type="button"
        class="w-50 mb-5 flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
      >
        Create New Topic
      </button>
      <!-- <button type="button" @click="showForm">Create New Topic</button> -->
    </div>
    <div v-if="showCreateFrom" class="mb-10" id="form-here">
      <form
        @submit.prevent="handleSubmit"
        class="max-w-lg mx-auto my-10 p-6 shadow-lg"
      >
        <div class="mb-6">
          <label for="name" class="block text-sm font-medium text-gray-700"
            >Name:</label
          >
          <input
            id="name"
            type="text"
            v-model="formData.name"
            class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
          />
        </div>

        <button
          type="submit"
          class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
          Submit
        </button>
      </form>
    </div>
    <template :key="index" v-for="(info, index) in userInfo">
      <DataComponent
        :info="info"
        :keyIndex="index"
        @deleteNode="deleteNode"
        @editNode="editNode"
      />
    </template>
  </div>
</template>

<script setup>
import { ref } from "vue";
import dataJson from "./assets/data.json";
import DataComponent from "./components/DataComponent.vue";

let userInfo = ref(dataJson.topics);
let showCreateFrom = ref(false);
const formData = ref({
  name: "",
  guid: null,
  comments: [],
});

function handleSubmit() {

  // if the form data guid field is null then meaning it is create
  if (!formData.value.guid) {
    const newName = formData.value.name;
    userInfo.value.unshift({
      name: newName,
      guid: userInfo.value.length + 10,
      comments: [],
    });
  } else {

    // else if guid is NOT null then meaning it is edit
    const index = userInfo.value.findIndex(
      (topic) => topic.guid === formData.value.guid
    );

    if (index !== -1) {
      const updatedName = formData.value.name;
      userInfo.value[index].name = updatedName;
    }
  }
  showForm();
}

const showForm = () => {
  showCreateFrom.value = !showCreateFrom.value;
  formData.value.name = null
  formData.value.guid = null
  formData.value.comments = []
};

const editNode = (data) => {
  showForm();
  formData.value.name = data.info.name;
  formData.value.guid = data.info.guid;
  formData.value.comments = data.info.comments;
};

const deleteNode = (dataSelected) => {
  const index = userInfo.value.findIndex(
    (topic) => topic.guid === dataSelected.info.guid
  );

  if (index !== -1) {
    userInfo.value.splice(index, 1);
  }
};
</script>
