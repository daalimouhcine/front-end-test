<template>
  <form @submit.prevent="submitData" class="space-y-3 my-10 mx-auto w-fit">
    <div class="flex gap-x-3">
      <label for="name">Name</label>
      <input
        class="rounded-md px-3 py-2"
        type="text"
        name="name"
        id="name"
        v-model="name"
      />
    </div>
    <div class="flex gap-x-3">
      <label for="price">Price</label>
      <input
        class="rounded-md px-3 py-2"
        type="number"
        name="price"
        id="price"
        v-model="price"
      />
    </div>
    <div class="flex gap-x-3">
      <label for="type">Type</label>
      <input
        class="rounded-md px-3 py-2"
        type="text"
        name="type"
        id="type"
        v-model="type"
      />
    </div>
    <button class="bg-zinc-700" type="submit">Send Data</button>
  </form>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const emits = defineEmits(["refetchData"]);

const name = ref("");
const price = ref(0);
const type = ref("");

const addRequest = async (newData) => {
  try {
    await axios.post("http://127.0.0.1:8000/api/data", newData);
    emits("refetchData");
  } catch (e) {
    console.log(e.message);
  }
};

const submitData = () => {
  const newData = {
    name: name.value,
    price: price.value,
    type: type.value,
  };

  addRequest(newData);

  name.value = "";
  price.value = 0;
  type.value = "";
};
</script>
