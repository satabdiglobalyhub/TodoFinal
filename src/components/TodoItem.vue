<template>
  <div class="todo-item">
    <div class="todo-item-left">
      <input type="checkbox" v-model="completed" @change="doneEdit" />
      <div
        class="todo-item-label"
        :class="{ completed: completed }"
        v-if="!editing"
        @dblclick="editTodo"
      >
        {{ title }}
      </div>
      <input
        type="text"
        v-model="title"
        v-else
        class="todo-item-edit"
        @blur="doneEdit"
        @keyup.enter="doneEdit"
        @keyup.esc="cancelEdit"
      />
    </div>
    <div class="remove-item" @click="removeTodo(index)">x</div>
  </div>
</template>

<script>
export default {
  name: "todo-item",
  props: {
    todo: {
      type: Object,
      require: true,
    },
    index: {
      type: Number,
      require: true,
    },
    checkAll: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      id: this.todo.id,
      title: this.todo.title,
      completed: this.todo.completed,
      editing: this.todo.editing,
      beforeEditCache: "",
    };
  },
  watch: {
    checkAll() {
      if (this.checkAll) {
        this.completed = true;
      } else {
        this.completed = this.todo.completed;
      }
    },
  },
  methods: {
    removeTodo(index) {
      this.$emit("removedTodo", index);
    },

    editTodo() {
      this.beforeEditCache = this.title;
      this.editing = true;
    },
    doneEdit() {
      if (this.title.trim().length == "") {
        this.title = this.beforeEditCache;
      }
      this.editing = false;
      this.$emit("finishedEdit", {
        index: this.index,
        todo: {
          id: this.id,
          title: this.title,
          completed: this.completed,
          editing: this.editing,
        },
      });
    },

    cancelEdit() {
      this.title = this.beforeEditCache;
      this.editing = false;
    },
  },
};
</script>
