<template>
  <div :class="task.isComplete ? 'taskCard-done' : 'taskCard-not-done'">
    <div v-bind="task" :key="task.id">
      <form @submit.prevent>
        <button id="xButton" @click="removeTask(task)" style="float: right">X</button>
        <div :class="task.isComplete ? 'done' : 'not-done'">
          <h2 v-if="titleFlag" @click="editTitle" >{{task.title}}</h2>
          <input v-model="task.title" v-if="!titleFlag" type="text" /><br/>
          <button v-if="!titleFlag" @click="editTitle">
            Change
          </button>
        </div>
        <div>
          <p v-html="task.description" @click="editDescription" v-if="descriptionFlag"></p>
          <textarea v-model="task.description" v-if="!descriptionFlag"></textarea>
          <br/>
          <button v-if="!descriptionFlag" @click="editDescription">
            Change
          </button>
        </div>
        <button @click="isCompleteMark(task)" style="margin: 0px 0px 10px; width: 95%">
          {{task.isComplete ? "Mark as: Done" : "Mark as: Running"}}
        </button>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
/* eslint-disable vue/no-async-in-computed-properties */
import axios from 'axios';
import { PropType, defineComponent, ref } from 'vue';
import { TodoItem } from '../types';

export default defineComponent({
  name: 'TodoItemC',
  props: {
    task: {
      type: Object as PropType<TodoItem>,
      required: true,
    },
  },
  setup(props, { emit }) {
    const titleFlag = ref(true);
    const descriptionFlag = ref(true);

    // changeTask
    const editTitle = () => {
      titleFlag.value = (!titleFlag.value);
      const tempTask = {
        id: props.task.id,
        title: props.task.title,
        description: props.task.description,
        isComplete: props.task.isComplete,
      };

      axios.put(`http://localhost:3000/todoitems/${tempTask.id}`, tempTask);
    };

    const editDescription = () => {
      descriptionFlag.value = (!descriptionFlag.value);
      const tempTask = {
        id: props.task.id,
        title: props.task.title,
        description: props.task.description,
        isComplete: props.task.isComplete,
      };

      axios.put(`http://localhost:3000/todoitems/${tempTask.id}`, tempTask);
    };

    const removeTask = (taskItem: TodoItem) => {
      emit('removeItem', taskItem.id);
    };

    return {
      titleFlag,
      descriptionFlag,
      editTitle,
      editDescription,
      removeTask,
    };
  },
  methods: {
    isCompleteMark(taskItem: TodoItem) {
      const tempTask = taskItem;
      tempTask.isComplete = !taskItem.isComplete;
      axios.put(`http://localhost:3000/todoitems/${taskItem.id}`, tempTask);
    },

  },
});
</script>

<style scoped>
.done{
  background-color: #63f794;
}
.not-done{
  background-color: #fcc203;
}
.taskCard-done{
  margin: 20px 5px 20px 5px;
  background-color: white;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  border-radius: 10px;
  border: 2px solid #63f794;
}
.taskCard-not-done{
  margin: 20px 5px 20px 5px;
  background-color: white;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  border-radius: 10px;
  border: 2px solid #fcc203;
}
button {
  background-color: grey;
  border: none;
  color: white;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 4px;
  cursor: pointer;
  border-radius: 5px;
}
input[type=text] {
  border: none;
  background-color: transparent;
  font-size: 20px;
  font-weight: bold;
  border-bottom: 2px solid black;
  width: 200px;
}
textarea {
  border: none;
  width: 90%;
  height: 100px;
  border: 2px solid transparent;
  border-radius: 0px 0px 5px 5px;
  margin: 0px 10px 5px 10px;
}
h2 {
  margin: 0;
  min-width: 300px;
}
#xButton{
  border-radius: 5px;
  background-color: #fc7e65;
}

</style>
