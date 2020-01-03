<template>
  <div>
    <div class="colorbar"></div>
    <md-field class="addTodoBox" style="margin: 0">
      <md-input v-model="currentTodo" @keydown.enter="addTodo()" placeholder="Add a todo" style="font-size: 7em; height: 1.5em"></md-input>
    </md-field>
    <div class="colorbar"></div>
    <ul class="todos">
      <li v-for="todo in todos" :key="todo.id">
        <input
          class="editing"
          type="text"
          id="editor"
          :placeholder="todo.label"
          v-if="todo.editedTodo"
          @blur="unEditTodo(todo)"
          @keydown.enter="recordTodo(todo)"
        >
        <div v-if="!todo.editedTodo">
          <input class="box" type="checkbox" @click="toggleTodo(todo)">
          <span v-if="!todo.completed" @dblclick="editTodo(todo)">{{ todo.label }}</span>
          <strike v-if="todo.completed">{{ todo.label }}</strike>
          <button class="button" @click="removeTodo(todo)">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        todos: [],
        currentTodo: ''
      };
    },
    methods: {
      addTodo() {
        this.todos.push({id: this.todos.length, label: this.currentTodo, completed: false, editedTodo: false});
        this.currentTodo = '';
      },
      toggleTodo(todo){
        var index = this.todos.indexOf(todo);
        this.todos[index].completed = !this.todos[index].completed;
      },
      removeTodo(todo){
        var index = this.todos.indexOf(todo);
        this.todos.splice(index, 1);
        let x = 0;
        this.todos.forEach(n =>
          {
            n.id = x;
            x++;
          }
        );
      },
      editTodo(todo){
        var index = this.todos.indexOf(todo);
        this.todos[index].editedTodo = !this.todos[index].editedTodo;
        setTimeout(function(){document.getElementById("editor").focus();}, 100);
      },
      unEditTodo(todo){
        var index = this.todos.indexOf(todo);
        this.todos[index].editedTodo = false;
      },
      recordTodo(todo){
        var index = this.todos.indexOf(todo);
        this.todos[index].label = document.getElementById("editor").value;
        this.unEditTodo(todo);
      }
    }
  };
</script>

<style>

body{
  background-color: #001f3f;
  color: 01FF70;
}

.md-field{
  background: #FFFFF1;
}

.colorbar{
  width: 100%;
  height: 20px;
  background-color: #450810;
}

.addTodoBox{
  padding-left: 20px;
}

ul {
  margin: 10px 50px;
  color: #FFFFF1;
  font-size: 2.1em;
}

li {
  padding: 8px;
  list-style-type: none;
  height: 2em;
}

input {
  height: 2em;
}

span, strike {
  padding: 5px 20px 5px 8px;
}

.button {
  background: #3498db;
  width: 120px;
  height: 18px;
  padding: 0;
  border-radius: 7px;
  text-transform: uppercase;
  color: #EED;
  display: inline-block;
  text-align: center;
}

button:hover {
  cursor: pointer;
}

.box {
  cursor: pointer;
  height: 18px;
  width: 18px;
}

</style>
