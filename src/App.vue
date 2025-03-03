<script setup>
import { ref } from 'vue';
import { watch } from 'vue';

const name = ref('');
const date = ref(new Date().toISOString().substring(0, 10));
const done = ref(false);
const msg = 'Todo App';
const itemButton = ref(false);

class item {
  constructor(name, date, done) {
  this.name = name;
  this.date = date;
  this.done = false;
  }
}

const itemList = ref(
  JSON.parse(localStorage.getItem('itemList'))?.map(item => ({
    name: item.name,
    date: item.date,
    done: item.done
  })) || []
);

function addItem(name, date){
  let newItem = new item(name, new Date(date).toISOString().substring(0, 10), false);
  itemList.value.push(newItem);
}

watch(itemList, (newList) => {
  localStorage.setItem('itemList', JSON.stringify(newList));
}, { deep: true });
</script>

<template>
  <h1>
    {{ msg }}
  </h1>
  <p>
    <button @click="itemButton=true; console.log({itemButton})" class="add_button">Add Item</button>
    <section v-if="itemButton">
      <label for="userInput">Enter name:</label>
      <input type="text" v-model="name" id="name" />
      <label for="userInput">Enter date:</label>
      <input type="date" v-model="date" id="date" />
      <button @click="addItem(name, date, false); itemButton=false; console.log({itemButton})" class="add_buttons">Add</button>
      <button @click="itemButton=false" class="add_buttons">Cancel</button>
    </section>
  </p>

  <li v-for="(item, index) in itemList" :key="index">>
    <input type="checkbox" class="myCheckbox" v-model="item.done">
    <label for="myCheckbox" class="status-text">
      <em>{{ item.name }}</em>  {{ item.date }}
      <button @click="itemList.splice(index,1)" class="delete_button">Delete</button>
    </label>
  </li>

</template>

<style>
em{
  font-weight: bold;
  font-style: normal;
}

.status-text {
  color: red;
}

input[type="checkbox"]:checked + .status-text {
  color: green;
}

input[type="checkbox"] {
  margin-right: 5px;
}

input[type="text"] {
  margin-right: 5px;
  margin-left: 5px;
}

input[type="date"] {
  margin-left: 5px;
}

.add_button {
  width: 100px;
  height: 30px;
  border-radius: 10px;
  border-color: white;
  margin-bottom: 10px;
}

.add_buttons {
  margin-left: 5px;
}

</style>
