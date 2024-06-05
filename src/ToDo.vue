<template>
  <main class="grid">
    <div class="applicationContainer">
      <div class="textContainers">
        <div class="textContainer1">
          <p id="datetime">{{ datetimeArray[0] }}</p>
          <p id="datetime2">{{ dateName }}</p>
        </div>
        <div class="textContainer2">
          <p id="taskText">Task</p>
          <p id="flowText">Flow</p>
        </div>
      </div>
      <div class="inputBox">
        <input v-model="newTodo" placeholder="Add a new task" type="text"/>
        <button class="addTodo" @click="addTodo">+</button>
      </div>
      <div class="todoListBox">
        <p class="numberTodos">Task to do  - {{ numberOfTodo }}</p>
        <ul>
          <li v-for="todo in todos" :key="todo.id" class="toDos">
            {{ todo.text }}
            <div class="todoEditButtons">
              <button class="toDoButtonCheck">
                <i class="fa-solid fa-check"></i>
              </button>
              <button class="toDoButtonCheck">
                <i class="fa-solid fa-pen"></i>
              </button>
              <button class="toDoButtonCheck">
                <i class="fa-solid fa-trash"></i>
              </button>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </main>
</template>

<script>
import {computed, ref} from 'vue';

export default {
  setup() {
    const todos = ref([]);
    const newTodo = ref('');

    const addTodo = () => {
      if (newTodo.value.trim()) {
        todos.value.push({
          id: Math.random().toString(36).slice(2, 7),
          text: newTodo.value,
          completed: false,
        });
        newTodo.value = '';
      }
    };

    const now = new Date();
    const datetime = now.toLocaleString();
    const datetimeArray = ref(datetime.split("/", 3));
    const month = datetimeArray.value[1].split("/")[0];
    let dateName = ref("");

    switch (month) {
      case "01":
        dateName.value = "Jan";
        break;
      case "02":
        dateName.value = "Feb";
        break;
      case "03":
        dateName.value = "Mar";
        break;
      case "04":
        dateName.value = "Apr";
        break;
      case "05":
        dateName.value = "May";
        break;
      case "06":
        dateName.value = "Jun";
        break;
      case "07":
        dateName.value = "Jul";
        break;
      case "08":
        dateName.value = "Aug";
        break;
      case "09":
        dateName.value = "Sep";
        break;
      case "10":
        dateName.value = "Oct";
        break;
      case "11":
        dateName.value = "Nov";
        break;
      case "12":
        dateName.value = "Dec";
        break;
    }

    const numberOfTodo = computed(() => todos.value.length);

    return {
      numberOfTodo,
      todos,
      newTodo,
      addTodo,
      datetimeArray,
      dateName,
    };
  }
};
</script>
