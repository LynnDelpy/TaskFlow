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

        <input v-model="newTodo" placeholder="Add a new task" type="text" id="addTodo" @focus="searchBoxClicked"/>
        <button class="plusButton" @click="addTodo">+</button>
        <div class="modal-fade" id="addTaskPopup" tabindex="-1" aria-labelledby="addTaskPopupLabel" aria-hidden="true">
          <div class="modal-dialog">
            <div class="modal-content" v-html="popupTemplate"></div>
          </div>
        </div>
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
import { ref, onMounted } from 'vue';
import { Modal } from 'bootstrap';

export default {
  setup() {
    const todos = ref([]);
    const newTodo = ref('');
    const newTodoDescription = ref('');
    const isPopupVisible = ref(false);

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

    const searchBoxClicked = () => {
      const modal = new Modal(document.getElementById('addTaskPopup'));
      isPopupVisible.value = true;
      modal.show();
    };

    const popupTemplate = `
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">Add New Task</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <input v-model="newTodo" type="text" placeholder="Task Name">
          <input v-model="newTodoDescription" type="text" placeholder="Task Description">
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
          <button type="button" class="btn btn-primary" @click="addTodo">Add</button>
        </div>
      </div>
    `;

    onMounted(() => {
      const inputBox = document.getElementById("addTodo");
      if (inputBox) {
        inputBox.addEventListener('focus', searchBoxClicked);
      }
    });

    const now = new Date();
    const datetime = now.toLocaleString();
    const datetimeArray = datetime.split("/", 2);
    let dateName = "";

    switch (datetimeArray[1]) {
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

    return {
      todos,
      newTodo,
      newTodoDescription,
      addTodo,
      searchBoxClicked,
      datetimeArray,
      dateName,
      isPopupVisible,
      popupTemplate,
    };
  }
};

</script>
