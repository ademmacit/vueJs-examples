<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";
import axios from "axios";
export default {
  name: "Home",
  components: {
    Todos,
    AddTodo,
  },
  data() {
    return {
      todos: [],
    };
  },
  methods: {
    //for some reason when deleting an added todo all added todos get deleted
    deleteTodo(id) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then((this.todos = this.todos.filter((todo) => todo.id !== id)))
        .catch((err) => console.log(err));
    },
    addTodo(newTodo) {
      //strips the argument todo
      const { title, completed } = newTodo;
      //axios is an http library. fetch can be used as well
      //jsonplace holder is an mock rest api that can be used for tests
      axios
        .post("https://jsonplaceholder.typicode.com/todos", {
          title,
          completed,
        })
        .then((res) => (this.todos = [...this.todos, res.data]))
        .catch((err) => console.log(err));
    },
  },
  //created is called when the component is created
  created() {
    axios
      .get("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then((res) => (this.todos = res.data))
      .catch((err) => console.log(err));
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
.btn:hover {
  background: #666;
}
</style>