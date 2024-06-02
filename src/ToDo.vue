<template>
  <main class="grid">
    <div class="applicationContainer">
      <div class="textContainer1">
        <p id="datetime">{{ datetimeArray[0] }}</p> <br><br>
        <p id="datetime2">{{ datetimeArray[1] }}</p>
      </div>
      <div class="textContainer2">

      </div>
      <div class="inputBox">
        <input v-model="newTodo" placeholder="Enter a To-Do item..." type="text"/>
        <button @click="addTodo">Add To-Do</button>
      </div>
      <div class="todoListBox">
        <ul>
          <li v-for="todo in todos" :key="todo.id">
            {{ todo.text }}
          </li>
        </ul>
      </div>
    </div>
  </main>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const todos = ref([]);
    const newTodo = ref('');

    const addTodo = () => {
      if (newTodo.value.trim()) { // Check for empty input
        todos.value.push({
          id: Math.random().toString(36).slice(2, 7), // Generate unique ID
          text: newTodo.value,
          completed: false,
        });
        newTodo.value = '';
      }
    };

    const now = new Date();
    const datetime = now.toLocaleString();
    const datetimeArray = datetime.split("/", 2);

    return { todos, newTodo, addTodo, datetimeArray }; // Include datetime in setup
  }
};
</script>

