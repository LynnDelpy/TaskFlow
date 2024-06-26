<template>
  <div v-if="visible" class="popupOverlay">
    <div class="popupContent">
      <h2>{{ isEditing ? 'Edit Todo' : 'Add New Todo' }}</h2>
      <div class="inputBox">
        <label for="title">Title:</label>
        <input id="title" v-model="inputs.title" placeholder="Enter title" required type="text"/>
      </div>
      <div class="inputBox">
        <label for="description">Description:</label>
        <textarea id="description" v-model="inputs.description" placeholder="Enter description" required></textarea>
      </div>
      <div class="inputBox">
        <label for="author">Author:</label>
        <input id="author" v-model="inputs.author" placeholder="Enter author" required type="text"/>
      </div>
      <div class="inputBox">
        <label for="category">Category:</label>
        <select id="category" v-model="inputs.category" required>
          <option value="" disabled>Select category</option>
          <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
        </select>
      </div>
      <div class="inputBox">
        <label for="important">Important:</label>
        <input id="important" v-model="inputs.important" type="checkbox"/>
      </div>
      <div class="inputBox">
        <label for="urgent">Urgent:</label>
        <input id="urgent" v-model="inputs.urgent" type="checkbox"/>
      </div>
      <div class="inputBox">
        <label for="startdate">Start Date:</label>
        <input id="startdate" v-model="inputs.startDate" required type="date"/>
      </div>
      <div class="inputBox">
        <label for="enddate">End Date:</label>
        <input id="enddate" v-model="inputs.endDate" required type="date"/>
      </div>
      <button class="submitTodo" @click="submitTodo">{{ isEditing ? 'Save' : 'Submit' }}</button>
      <button class="closePopup" @click="closePopup">Close</button>
    </div>
  </div>
</template>

<script>
import { ref, watch, onMounted } from 'vue';

export default {
  props: {
    visible: {
      type: Boolean,
      required: true
    },
    todoData: {
      type: Object,
      default: () => ({})
    }
  },
  emits: ['close', 'submit'],
  setup(props, { emit }) {
    const inputs = ref({
      title: '',
      description: '',
      author: '',
      category: '',
      important: false,
      urgent: false,
      startDate: '',
      endDate: ''
    });

    const categories = ref(['Work', 'Personal', 'Others']);

    const isEditing = ref(false);

    watch(() => props.todoData, (newVal) => {
      if (Object.keys(newVal).length) {
        inputs.value = { ...newVal };
        isEditing.value = true;
      } else {
        inputs.value = {
          title: '',
          description: '',
          author: '',
          category: '',
          important: false,
          urgent: false,
          startDate: '',
          endDate: ''
        };
        isEditing.value = false;
      }
    }, { immediate: true });

    const submitTodo = () => {
      if (inputs.value.title && inputs.value.description && inputs.value.author && inputs.value.category && inputs.value.startDate && inputs.value.endDate) {
        emit('submit', { ...inputs.value });
        closePopup();
      } else {
        alert('Please fill all the required fields');
      }
    };

    const closePopup = () => {
      emit('close');
    };

    return {
      inputs,
      categories,
      submitTodo,
      closePopup,
      isEditing
    };
  }
};
</script>
