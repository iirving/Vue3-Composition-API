<script setup>
import { ref, computed } from 'vue'

const header = ref('Shopping List App');
const editing = ref(false);
const items = ref([
  {
    id: 5,
    label: "10 party hats",
    highPriority: false,
    purchased: true
  },
  {
    id: 2,
    label: "2 board games",
    highPriority: false,
    purchased: true
  },
  {
    id: 3,
    label: "20 cups",
    highPriority: true,
    purchased: false
  }
]);
const newItem = ref("");
const newItemHighPriority = ref(false);

/**
 * Computes the character count of the newItem value.
 *
 * @returns {number} The character count of the newItem value.
 */
const characterCount = computed(() => {
  return newItem.value.length;
});

/**
 * Computed property that returns a reversed copy of the items array.
 *
 * @returns {Array} The reversed items array.
 */
const reverseItemsArray = computed(() => [...items.value].reverse());

/**
 * Saves a new item to the items array.
 */
const saveItem = () => {
  items.value.push({
    id: items.value.length + 1,
    label: newItem.value,
    highPriority: newItemHighPriority.value,
    purchased: false
  });
  newItem.value = "";
  newItemHighPriority.value = false;
}

/**
 * Toggles the editing state and resets the newItem and newItemHighPriority values.
 */
const doEdit = () => {
  editing.value = !editing.value;
  newItem.value = "";
  newItemHighPriority.value = false;
}

/**
 * Toggles the purchased status of an item.
 * @param {Object} item - The item to toggle the purchased status for.
 */
const togglePurchased = (item) => {
  item.purchased = !item.purchased;
}
</script>

<template>
  <div>
    <h1>{{ header || 'Welcome' }}</h1>
    <button class="btn btn-primary" @click="doEdit">
      {{ editing ? 'Cancel' : 'Add item' }}
    </button>
  </div>
  <form class="add-item-form" v-if="editing" @submit.prevent="saveItem">

    <input type="text" placeholder="Add an item." v-model.trim="newItem">

    <label>
      <input type="checkbox" v-model="newItemHighPriority" value="high">
      High Priority
    </label>

    <button :disabled="newItem.length < 5" class="btn btn-primary">
      Save
    </button>


  </form>
  <div v-if="editing">
    {{ characterCount }} characters
  </div>
  <ul>
    <li v-for="({ id, label, purchased, highPriority }, index) in reverseItemsArray" :key="id"
      @click="togglePurchased(reverseItemsArray[index])" :class="{
        strikeout: purchased,
        'static-class': true,
        priority: highPriority
      }">
      {{ label }}
    </li>
  </ul>
  <p v-if="items.length === 0">No items yet.</p>
</template>
