<template>
  <div class="taskList">
    <h1>Task list:</h1>
    <div v-for="task in getAllTasks" :key="task.id">
      <TodoItemC :task="task" @removeItem="removeItem"/>
    </div>
    <div v-for="task in getAllTasks" :key="task.id"></div>
  </div>
</template>

<script lang="ts">
import { ref } from 'vue';
import axios from 'axios';
import { TodoItem } from '../types';
import TodoItemC from './TodoItem.vue';
import { PropType } from 'vue';

export default {
  components: { TodoItemC },

  props: {
    task: {
      type: Object as PropType<TodoItem>,
      required: true,
    },
  },

  setup() {
    const getAllTasks = ref<TodoItem[]>([]);
    axios.get('http://localhost:3000/todoitems').then((response) => {
      console.log(response.data);
      getAllTasks.value = response.data;
    });

    const removeItem = (id: string) => {
      axios.delete(`http://localhost:3000/todoitems/${id}`);
      axios.get('http://localhost:3000/todoitems').then((response) => {
        getAllTasks.value = response.data;
      });
    };

    return {
      getAllTasks,
      removeItem,
    };
  },
};
</script>

<style scoped>
.taskList{
  background-color: #f2f2f2;
  padding: 10px;
  border-radius: 5px;
  max-width: 500px;
}
</style>
