<template>
  <main class="grid">
    <div class="applicationContainer">
      <div class="textContainers">
        <div class="textContainer1">
          <p id="datetime">{{ datetimeArray[0] }}</p>
          <p id="datetime2">{{ dateNameRef }}</p>
        </div>
        <div class="textContainer2">
          <p id="taskText">Task</p>
          <p id="flowText">Flow</p>
        </div>
      </div>
      <div class="inputBox">
        <input v-model="newTodo" placeholder="Add a new task" type="text"/>
        <button class="addTodo" @click="openPopup()">+</button>
      </div>
      <div class="todoListBox">
        <p class="numberTodos">Tasks to do - {{ numberOfTodo }}</p>
        <ul>
          <li v-for="todo in pendingTodos" :key="todo.id" class="toDos">
            <div v-if="todo.type === 'todo'" class="todoContent">
              <span class="todoTitle">
                <i v-if="todo.urgent" id="urgent" class="fa-solid fa-circle-exclamation"></i>
                <i v-if="todo.important" id="important" class="fa-solid fa-clock"></i>
                {{ todo.title }}
              </span>
              <div class="todoEditButtons">
                <button aria-label="Mark as done" class="toDoButtonCheck" @click="markAsDone(todo.id)">
                  <i class="fa-solid fa-check"></i>
                </button>
                <button aria-label="Edit task" class="toDoButtonCheck" @click="editTodo(todo)">
                  <i class="fa-solid fa-pen"></i>
                </button>
                <button aria-label="Delete task" class="toDoButtonCheck" @click="removeTodo(todo.id)">
                  <i class="fa-solid fa-trash"></i>
                </button>
              </div>
            </div>
          </li>
        </ul>
        <p id="percentDone" class="numberTodos">Tasks done - {{ doneTodoPercentage }}%</p>
        <div class="doneTodos">
          <ul>
            <li v-for="todo in doneTodos" :key="todo.id" class="toDos done">
              <div class="todoContent">
                <span class="todoTitle">{{ todo.title }}</span>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <Popup
        :todoData="isEditing ? currentTodo : {}"
        :visible="showPopup"
        @close="showPopup = false"
        @submit="handleSubmit"
    />
  </main>
</template>

<script>
import {computed, ref} from 'vue';
import Popup from "/src/components/Popop.vue";

export default {
  components: {
    Popup
  },
  setup() {
    const todos = ref([]);
    const newTodo = ref('');
    const showPopup = ref(false);
    const isEditing = ref(false);
    const currentTodo = ref(null);

    const addTodo = (todoData) => {
      const todo = {
        id: Math.random().toString(36).substr(2, 9),
        type: 'todo',
        ...todoData,
        completed: false,
      };
      todos.value.push(todo);
    };

    const handleSubmit = (todoData) => {
      if (isEditing.value) {
        const index = todos.value.findIndex(todo => todo.id === currentTodo.value.id);
        todos.value[index] = {...currentTodo.value, ...todoData};
      } else {
        addTodo(todoData);
      }
      showPopup.value = false;
      isEditing.value = false;
    };

    const editTodo = (todo) => {
      currentTodo.value = {...todo};
      showPopup.value = true;
      isEditing.value = true;
    };

    const removeTodo = (id) => {
      todos.value = todos.value.filter(todo => todo.id !== id);
    };

    const markAsDone = (id) => {
      const todo = todos.value.find(todo => todo.id === id);
      if (todo) {
        todo.completed = true;
      }
    };

    const getDateTime = () => {
      const now = new Date();
      const dateArray = now.toLocaleString().split("/", 3);
      const month = dateArray[1];
      let dateName = '';
      switch (month) {
        case "01":
          dateName = "Jan";
          break;
        case "02":
          dateName = "Feb";
          break;
        case "03":
          dateName = "Mar";
          break;
        case "04":
          dateName = "Apr";
          break;
        case "05":
          dateName = "May";
          break;
        case "06":
          dateName = "Jun";
          break;
        case "07":
          dateName = "Jul";
          break;
        case "08":
          dateName = "Aug";
          break;
        case "09":
          dateName = "Sep";
          break;
        case "10":
          dateName = "Oct";
          break;
        case "11":
          dateName = "Nov";
          break;
        case "12":
          dateName = "Dec";
          break;
      }
      return {dateArray, dateName};
    };

    const {dateArray, dateName} = getDateTime();
    const datetimeArray = ref(dateArray);
    const dateNameRef = ref(dateName);

    const numberOfTodo = computed(() => todos.value.filter(todo => !todo.completed).length);
    const pendingTodos = computed(() => todos.value.filter(todo => !todo.completed));
    const doneTodos = computed(() => todos.value.filter(todo => todo.completed));
    const doneTodoPercentage = computed(() => {
      const totalTodos = todos.value.length;
      const doneCount = doneTodos.value.length;
      return totalTodos === 0 ? 0 : Math.round((doneCount / totalTodos) * 100);
    });

    const openPopup = () => {
      newTodo.value = '';
      showPopup.value = true;
    };


    return {
      numberOfTodo,
      todos,
      newTodo,
      showPopup,
      isEditing,
      currentTodo,
      addTodo,
      handleSubmit,
      editTodo,
      removeTodo,
      markAsDone,
      datetimeArray,
      dateNameRef,
      pendingTodos,
      doneTodos,
      doneTodoPercentage,
      openPopup
    };
  }
};
</script>

