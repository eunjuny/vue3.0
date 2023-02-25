<template>
  <div class="container">
    <h2>To-Do List</h2>
    <input
      class="form-control"
      type="text"
      v-model="searchText"
      placeholder="Search"
    />
    <hr />
    <TodoSimpleForm @add-todo="addTodo" />

    <div v-if="!filteredTodos.length">
      검색 결과가 없습니다.
    </div>
    <TodoList
      :todos="filteredTodos"
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
  </div>
</template>

<script>
import { ref, computed } from "vue";
import TodoSimpleForm from "./components/TodoSimpleForm.vue";
import TodoList from "./components/TodoList.vue";
import axios from 'axios';

export default {
  components: {
    TodoSimpleForm,
    TodoList,
  },
  setup() {
    const todos = ref([]);

    const getTodos = async () => {
      try{
      const res = await axios.get('http://localhost:3000/todos');
        todos.value = res.data;
      } catch (err){
        console.log(err);
      }
    };

    getTodos();

    const addTodo = async (todo) => {
      try{
        const res = await axios.post('http://localhost:3000/todos',{
          subject: todo.subject,
          completed: todo.completed,
        });
        todos.value.push(res.data);
      }catch(err){
        console.log(err);
      
      }
      // axios.post('http://localhost:3000/todos',{
      //   subject: todo.subject,
      //   completed: todo.completed,
      // }).then(res => {
      //   console.log(res);
      //   todos.value.push(res.data);
      // }).catch(err => {
      //   console.log(err);
      // })
      
    };

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    };

    const toggleTodo = (index) => {
      console.log(index);
      todos.value[index].completed = !todos.value[index].completed;
    };

    const searchText = ref("");
    const filteredTodos = computed(() => {
      if(searchText.value) {
        return todos.value.filter(todo => {
          return todo.subject.includes(searchText.value);
        });
      }
      return todos.value;
    })

    return {
      todos,
      addTodo,
      deleteTodo,
      toggleTodo,
      searchText,
      filteredTodos,
      getTodos,
    };
  },
};
</script>

<style>
.todo {
  color: gray;
  text-decoration: line-through;
}
</style>
