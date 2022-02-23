<template>
  <div style="padding: 0px 20% 0px 20%">
    <span style="float: left"><CreateTodoItem @addItem="addItem"/></span>
    <span style="float: left; margin: 0px 0px 0px 15%"><ShowTodoItems/></span>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';
import CreateTodoItem from '../components/CreateTodoItem.vue';
import ShowTodoItems from '../components/ShowTodoItems.vue';
import { TodoItem } from '../types';

export default defineComponent({
  components: {
    CreateTodoItem,
    ShowTodoItems,
  },

  setup() {
    const addItem = (task: TodoItem) => {
      console.log('i run');// not working
      axios.post('http://localhost:3000/todoitems', task);
      axios.get('http://localhost:3000/todoitems');
    };

    return {
      addItem,
    };
  },

});
</script>

<style scoped>

</style>
