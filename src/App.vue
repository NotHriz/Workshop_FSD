<script setup>
import PocketBase from 'pocketbase';
import { ref, onMounted } from 'vue';

const db = new PocketBase('http://127.0.0.1:8090');

const titleVal = ref("");
const descriptionVal = ref(""); // Define descriptionVal
const data = ref([]);

async function submit() {
  const recordData = {
    title: titleVal.value,
    description: descriptionVal.value,
  };
  const result = await db.collection("table").create(recordData);
  
  // Add the new record to the data list directly
  data.value.push(result);

  // Clear input fields after submission
  titleVal.value = "";
  descriptionVal.value = "";
}

onMounted(async () => {
  const result = await db.collection("table").getFullList(); // Added missing parentheses
  data.value = result;
});
</script>

<template>
  <div class="bg-white dark:bg-slate-800 rounded-lg px-6 py-8 ring-1 ring-slate-900/5 shadow-xl">  
    <input v-model="titleVal" placeholder="Title" class="border border-gray-300 rounded px-3 py-2 mb-3 w-full">
    <input v-model="descriptionVal" placeholder="Description" class="border border-gray-300 rounded px-3 py-2 mb-3 w-full">
    <br>
    <button class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded" @click="submit">Submit</button>
 
    <ul>
      <li v-for="item in data" :key="item.id" class="mt-4 border-b border-gray-300 pb-4">
        <p class="text-gray-700 font-bold">Title: {{ item.title }}</p>
        <p class="text-gray-500">Description: {{ item.description }}</p>
      </li>
    </ul>
  </div>
</template>

<style scoped>
</style>
