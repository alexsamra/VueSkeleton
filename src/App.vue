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
      {{ item.name }} - {{ item.date }}
      <button @click="itemList.splice(index,1)">Delete</button>
    </label>
  </h4>

</template>

