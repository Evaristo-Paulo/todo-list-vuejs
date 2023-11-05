<script setup>
import { ref, computed } from 'vue'

const header = ref('To-do List')
const editing = ref(false)
const items = ref([
  {
    id: 1,
    label: "Beginner vue.js course",
    purchased: true,
    highPriority: true
  },
  
  {
    id: 2,
    label: "Intermediate vue.js course",
    purchased: false,
    highPriority: false
  },
])
const newItem = ref("")
const reversedItems = computed(() => [...items.value].reverse())
const newItemHighPriority = ref(false)
const saveItem = () => {
  items.value.push({ id: items.value.length + 1, label: newItem.value, purchased: false, highPriority: newItemHighPriority.value })
  newItem.value = ""
  newItemHighPriority.value = false
}
const doEdit = (e) => {
  editing.value = e
}

const togglePurchased = (item) =>{
  item.purchased = !item.purchased
}

</script>


<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" @click="doEdit(false)" class="btn">Cancel</button>
    <button v-else @click="doEdit(true)" class="btn btn-primary">Add item</button>
  </div>
  <form v-if="editing" class="add-item-form" @submit.prevent="saveItem">
    <input type="text" v-model.lazy="newItem" placeholder="Add new item">
    <label>
      <input type="checkbox" v-model="newItemHighPriority"> High Priority
    </label>
    <button :disabled="newItem.length < 5" class="btn btn-primary">Save item</button>
  </form>
  <p v-if="!items.length">
    Nothing to see here
  </p>
  <ul v-else>
    <li v-for="item in reversedItems" :key="item.id"
      :class="{ strikeout: item.purchased, priority: item.highPriority }" @click="togglePurchased(item)">{{ item.label }}</li>
  </ul>
</template>

<style scoped>
</style>
