<script setup>
import { ref } from 'vue'

const header = ref('Shopping List App');
const items = ref([
  { id: 5, label: "10 party hats" },
  { id: 2, label: "2 board games" },
  { id: 3, label: "20 cups" }
]);
let newItem = ref("");
const newItemHighPriority = ref(false);
const saveItem = () => {
  items.value.push({ id: items.value.length + 1, label: newItem.value });
  newItem.value = "";
}
</script>

<template>
  <h1>{{ header || 'Welcome' }}</h1>
  <form class="add-item-form" @submit.prevent="saveItem">

    <input type="text" placeholder="Add an item." v-model.trim="newItem">

    <label>
      <input type="checkbox" v-model="newItemHighPriority" value="high">
      High Priority
    </label>
    <button class="btn btn-primary">
      save
    </button>
    <br>
    {{ newItemHighPriority }}
  </form>
  <ul>
    <li v-for="item in items" :key="item.id"> {{ item.label }}</li>
  </ul>
</template>
