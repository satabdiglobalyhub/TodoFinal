<template>
  <div>
    <input
      type="text"
      class="todo-input"
      placeholder="What needs to be done?"
      v-model="newTodo"
      @keyup.enter="addTodo"
      required
    />
    <todo-item
      v-for="(todo, index) in todosFiltered"
      :key="todo.id"
      :todo="todo"
      :index="index"
      :checkAll="!anyRemaining"
      @removedTodo="removeTodo"
      @finishedEdit="finishedEdit"
    >
    </todo-item>

    <div class="extra-container">
      <div>
        <label>
          <input
            type="checkbox"
            :checked="!anyRemaining"
            @change="checkAllTodos($event)"
          />
          Check All
        </label>
      </div>
      <div>{{ remaining }} items left</div>
    </div>

    <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">
          All
        </button>

        <button
          :class="{ active: filter == 'active' }"
          @click="filter = 'active'"
        >
          Active
        </button>

        <button
          :class="{ active: filter == 'completed' }"
          @click="filter = 'completed'"
        >
          Completed
        </button>
      </div>
      <div>
        <button v-if="showClearCompletedButton" @click="clearCompleted">
          Clear Completed
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import TodoItem from "./TodoItem.vue";

export default {
  name: "TodoList",
  components: {
    TodoItem,
  },
  data() {
    return {
      newTodo: "",
      idForTodo: 1,
      beforeEditCache: "",
      filter: "all",
      todos: [],
    };
  },
 
  computed: {
    remaining() {
      return this.todos.filter((todo) => !todo.completed).length;
    },

    anyRemaining() {
      return this.remaining != 0;
    },

    todosFiltered() {
      if (this.filter == "all") {
        return this.todos;
      } else if (this.filter == "active") {
        return this.todos.filter((todo) => !todo.completed);
      } else if (this.filter == "completed") {
        return this.todos.filter((todo) => todo.completed);
      }
      return this.todos;
    },

    showClearCompletedButton() {
      return this.todos.filter((todo) => todo.completed).length > 0;
    },
  },

  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return;
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
      });
      this.newTodo = "";
      this.idForTodo++;
    },

    removeTodo(index) {
      this.todos.splice(index, 1);
    },

    checkAllTodos(event) {
      this.todos.forEach((todo) => (todo.completed = event.target.checked));
    },

    clearCompleted() {
      this.todos = this.todos.filter((todo) => !todo.completed);
    },

    finishedEdit(data) {
      this.todos.splice(data.index, 1, data.todo);
    },
  },
};
</script>

<style>
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
}
.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.remove-item {
  cursor: pointer;
  margin-left: 14px;
  padding-right: 5px;
}
.remove-item:hover {
  color: red;
}
.todo-item-left {
  display: flex;
  align-items: center;
}
.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}
.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: Arial, Helvetica, sans-serif;
}
.completed {
  text-decoration: line-through;
  color: grey;
}
.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}
button {
  font-size: 14px;
  background-color: white;
  appearance: none;
}
.active {
  background: lightgreen;
}
</style>
