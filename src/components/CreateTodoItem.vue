<template>
  <div class="taskCreate">
    <h1>Create a task!</h1>
    <form @submit.prevent>
      <div class="labelDiv"><label>Title:</label></div>
      <input type="text" v-model="newTask.title"/><br/>
      <div class="labelDiv"><label>Description:</label></div>
      <textarea cols="50" rows="10" v-model="newTask.description"></textarea><br/>
      <div>
        <button @click="addItem">Add</button> | <button>Cancel</button>
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { reactive, toRefs, defineComponent } from 'vue';
import { TodoItem } from '../types';

export default defineComponent({
  setup(props, { emit }) {
    const state = reactive({
      newTask: {
        id: `T${Math.ceil(Math.random() * 1000).toString()}`,
        title: '',
        description: '',
        isComplete: false,
      } as TodoItem,
      taskItems: [] as TodoItem[],
    });

    const addItem = () => {
      state.taskItems.push({
        ...state.newTask,
        isComplete: false,
      });
      emit('removeItem', state.newTask);
    };

    return {
      ...toRefs(state),
      addItem,
    };
  },
});
</script>

<style scoped>
.taskCreate{
  background-color: #f2f2f2;
  padding: 10px;
  border-radius: 5px;
}
input[type=text] {
  border: none;
  width: 90%;
  height: 30px;
  border: 2px solid #fcc203;
  border-radius: 0px 0px 5px 5px;
  margin: 10px 10px 20px 10px;
}
textarea {
  border: none;
  width: 90%;
  height: 100px;
  border: 2px solid #fcc203;
  border-radius: 0px 0px 5px 5px;
  margin: 10px 10px 2px 15px;
}
.labelDiv{
  border: none;
  width: 90%;
  border-radius: 5px 5px 0px 0px;
  margin: 10px 10px 0px 15px;
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
</style>
