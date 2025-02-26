<script setup>
import { ref } from 'vue';

const msg = 'Todo App';
const itemList = ref([]);
const itemButton = ref(false);

const name = ref('');
const date = ref(new Date().toISOString().substring(0, 10));
const done = ref(false);

const formatDate = (date) => {
  return new Date(date).toDateString();
};

function item(name, date, done){
  this.name = name;
  this.date = date;
  this.done = done;
}

function addItem(name, date, done){
  let newItem = new item(name, date, done);
  itemList.value.push(newItem);
}

</script>

<template>
  <h1 style="text-align: center;">
    {{ msg }}
  </h1>
  <h2>
    <div style="text-align: center;">
      <button @click="itemButton=true; console.log({itemButton})">Add Item</button>
    </div>
  </h2>
  <h3>
    <div style="text-align: center;">
      <section v-if="itemButton">
        <label for="userInput">Enter name:</label>
        <input type="text" v-model="name" id="name" />
        <br>
        <br>
        <label for="userInput">Enter date:</label>
        <input type="date" v-model="date" id="date" />
        <br>
        <br>
        <button @click="addItem(name, date, false); itemButton=false; console.log({itemButton})">Add</button>
      </section>
    </div>
  </h3>

  <h4 v-for="(item, index) in itemList" :key="index">>
    <input type="checkbox" id="myCheckbox" v-model="item.done">
    
    <label for="myCheckbox" 
      :style="{ border: `2px solid ${item.done ? 'green' : 'red'}` }">
      {{ item.name }} - {{ formatDate(date) }}
      <button @click="itemList.splice(index,1)">Delete</button>
    </label>
  </h4>

</template>

