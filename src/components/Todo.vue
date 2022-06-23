<template>
  <div :key="todo.id" v-for="todo in todos" class="todo">
    <div class="left">
      <i
        @click="toggleDone(todo.id)"
        class="fa-regular fa-circle"
        :class="todo.done && 'fa-solid fa-circle'"
      />
      <p
        @dblclick="editTodo(todo.id)"
        class="itemText"
        :class="todo.done && 'itemTextDone'"
      >
        {{ todo.text }}
      </p>
    </div>
    <i @click="deleteTodo(todo.id)" class="fa-regular fa-times" />
  </div>
</template>

<script>
export default {
  name: "Todo",
  props: {
    todos: Array,
  },
  methods: {
    toggleDone(id) {
      this.$emit("toggle-status", id);
    },
    editTodo(id) {
      this.$emit("toggle-edit", id);
    },
    deleteTodo(id) {
      this.$emit("toggle-delete", id);
    },
  },
};
</script>

<style lang="scss" scoped>
.todo {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1.5px solid gray;
}

.left {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}

i {
  margin-left: 10px;

  &:hover {
    cursor: pointer;
  }
}

.itemText {
  color: black;
  font-size: 16px;
  margin-left: 20px;

  &:hover {
    cursor: pointer;
  }
}

.itemTextDone {
  color: gray;
  font-size: 16px;
  margin-left: 20px;
  text-decoration: line-through;

  &:hover {
    cursor: pointer;
  }
}

.fa-regular {
  &.fa-times {
    margin-right: 20px;

    &:hover {
      cursor: pointer;
    }
  }
}
</style>
