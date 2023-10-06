<template>
  <ul class="">
    <div v-if="!openEdit" class="flex gap-x-5 justify-between">
      <li>{{ data.name }}</li>
      <li>{{ data.price }}</li>
      <li>{{ data.type }}</li>
      <div class="flex gap-x-2">
        <button @click="deleteData"><TrashIcon /></button>
        <button @click="toggleEdit"><PenIcon /></button>
      </div>
    </div>
    <div v-else class="flex gap-x-5 justify-between">
      <input class="px-3 rounded-md" v-model="data.name" />
      <input class="px-3 rounded-md" v-model="data.price" />
      <input class="px-3 rounded-md" v-model="data.type" />
      <div class="flex gap-x-2">
        <button @click="toggleEdit">Cancel</button>
        <button @click="updateData">Save</button>
      </div>
    </div>
  </ul>
</template>

<script setup>
import { ref } from "vue";
import Swal from "sweetalert2";
import TrashIcon from "../icons/TrashIcon.vue";
import PenIcon from "../icons/PenIcon.vue";
import axios from "axios";

const openEdit = ref(false);
const props = defineProps(["data"]);
const emits = defineEmits(["refetchData"]);

const updateRequest = async () => {
  try {
    console.log({
      name: props.data.name,
      price: props.data.price,
      type: props.data.type,
    });
    await axios.put(`http://127.0.0.1:8000/api/data/${props.data.id}`, {
      name: props.data.name,
      price: props.data.price,
      type: props.data.type,
    });
    toggleEdit();
    emits("refetchData");
    Swal.fire("Updated!", "Your file has been updated.", "success");
  } catch (e) {
    Swal.fire("Error", "An error has occurred", "error");
    console.log(e.message);
  }
};

const updateData = () => {
  Swal.fire({
    title: "Are you sure?",
    text: "You won't be able to revert this!",
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#3085d6",
    cancelButtonColor: "#d33",
    confirmButtonText: "Yes, update it!",
  }).then((result) => {
    if (result.isConfirmed) {
      updateRequest();
    }
  });
};

const deleteRequest = async () => {
  try {
    await axios.delete(`http://127.0.0.1:8000/api/data/${props.data.id}`);
    emits("refetchData");
    Swal.fire("Deleted!", "Your file has been deleted.", "success");
  } catch (e) {
    Swal.fire("Error", "An error has occurred", "error");
    console.log(e.message);
  }
};

const deleteData = () => {
  Swal.fire({
    title: "Are you sure?",
    text: "You won't be able to revert this!",
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#3085d6",
    cancelButtonColor: "#d33",
    confirmButtonText: "Yes, delete it!",
  }).then((result) => {
    if (result.isConfirmed) {
      deleteRequest();
    }
  });
};

const toggleEdit = () => {
  openEdit.value = !openEdit.value;
};
</script>
