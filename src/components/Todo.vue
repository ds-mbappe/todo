<template>
  <div :key="todo.id" v-for="todo in todos" class="todo">
    <div class="left">
      <i
        @click="$emit('toggle-status', todo.id)"
        class="fa-regular fa-circle"
        :class="todo.done && 'fa-solid fa-circle'"
      />
      <p
        @click="$emit('toggle-status', todo.id)"
        @dblclick="$emit('toggle-edit', todo.id)"
        class="itemText"
        :class="todo.done && 'itemTextDone'"
      >
        {{ todo.text }}
      </p>
    </div>
    <i @click="$emit('toggle-delete', todo.id)" class="fa-regular fa-times" />
  </div>
</template>

<script>
export default {
  name: "Todo",
  props: {
    todos: Array,
  },
  emits: ["toggle-delete", "toggle-edit", "toggle-status"],
};
</script>

<style lang="scss" scoped>
.todo {
  width: 100%;
  height: 4em;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid gray;
  margin: 0.5em;
  border-radius: 0.5em;
  transition: all 0.2s ease-in-out;

  &:hover {
    cursor: pointer;
    transform: scale(1.2);
    background-color: rgb(242, 171, 157);
  }
}

.left {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}

i {
  margin-left: 20px;

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
