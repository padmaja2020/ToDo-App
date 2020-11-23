<template>
  <div class="container">
    <div class="toolbar">
      <md-toolbar>
        <h1>ToDo List App</h1>
      </md-toolbar>
    </div>

    <div class="addTodo">
      <md-field>
        <md-input
          v-model="currentTodo"
          @keydown.enter.prevent="addTodo()"
          placeholder="Add a todo"
          v-bind:class="{ error: hasError }"
        ></md-input>
        <button v-on:click.prevent="addTodo" type="submit">Add</button>
      </md-field>
    </div>

    <div class="todos" v-if="showTodos()">
      <md-list>
        <md-list-item>
          <h3>My Todo Tasks</h3>
        </md-list-item>
        <md-list-item v-for="todo in todos" :key="todo.id">
          <input type="checkbox" v-model="todo.completed" />
          <span
            class="title"
            contenteditable="true"
            @dblclick="updateTask($event, todo)"
            v-on:keydown.enter="updateTask($event, todo)"
            v-on:blur="updateTask($event, todo)"
            v-on:click.stop.prevent="clickToEdit(todo)"
            v-show="editTodoId !== todo.id"
            :class="{ completed: todo.completed }"
          >
            {{ todo.label }}
          </span>
          <button @click="removeTodo(todo)">Remove</button>
        </md-list-item>
      </md-list>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      currentTodo: "",
      editTodoId: null,
      hasError: false,
    };
  },
  methods: {
    showTodos() {
      return this.todos.length > 0;
    },
    addTodo() {
      if (!this.currentTodo) {
        this.hasError = true;
        console.log("error");
        return;
      }

      this.hasError = false;
      this.todos.push({
        id: this.todos.length,
        label: this.currentTodo,
        completed: false,
      });
      this.currentTodo = "";
    },
    removeTodo(todo) {
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },

    updateTask(e, todo) {
      e.preventDefault();
      todo.label = e.target.innerText;
      e.target.blur();
    },
  },
};
</script>

<style>
.completed {
  text-decoration: line-through;
}
.title {
  display: inline-block;
  width: 200px;
  border: 1px solid transparent;
  padding: 8px;
  font-size: 16px;
  vertical-align: middle;
  white-space: normal;
}
.addTodo {
  width: 70%;
  height: 70px;
  border: 0.5px solid white;
  margin: auto;
  margin-bottom: 10px;
}
[contenteditable="true"]:focus {
  overflow: hidden;
  border: 1px solid transparent;
  -webkit-appearance: textfield;
  -moz-appearance: textfield;
  appearance: textfield;
  white-space: normal;
}
.todos {
  width: 60%;
  height: 60%;
  margin: 0 auto;
  padding-top: 0px;
  border: 1px solid #040404;
}

button {
  background-color: #3e3e3e;
  font-size: 14px;
  padding: 8px 20px;
  cursor: pointer;
  color: white;
}
.container {
  width: 100%;
  text-align: center;
}
.toolbar {
  background-color: #040404;
  margin-bottom: 20px;
}
.title:hover {
  border: 1px solid #c4c4c4;
  border-radius: 10px;
}

body {
  background-color: #3e3e3e;
  color: white;
  font-size: 20px;
}
</style>
