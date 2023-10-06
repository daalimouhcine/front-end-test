<template>
  <h1>Home</h1>
  <FormData @refetchData="fetchData" />
  <div class="space-y-5">
    <DataDetails
      v-for="data in datas"
      :key="data.id"
      :data="data"
      @refetchData="fetchData"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import FormData from "../components/FormData.vue";
import DataDetails from "../components/DataDetails.vue";
import axios from "axios";

const datas = ref([]);

const fetchData = async () => {
  try {
    const response = await axios.get("http://127.0.0.1:8000/api/data");
    datas.value = response.data.datas;
  } catch (e) {
    console.log(e.message);
  }
};

onMounted(() => {
  fetchData();
});
</script>
