<template>
  <div class="card">
    <input
      class="search"
      type="text"
      placeholder="   What needs to be done ?"
      v-model="todoToAdd"
      @keyup.enter="addTodo(todoToAdd)"
    />
    <Todo
      @toggle-delete="toggleDelete"
      @toggle-edit="toggleEdit"
      @toggle-status="toggleStatus"
      :todos="todos"
    />
    <div class="footer">
      <p
        v-if="todos.filter((item) => item.done !== true).length > 1"
        class="leftItem"
      >
        {{ todos.filter((item) => item.done !== true).length }} items left
      </p>
      <p v-else class="leftItem">
        {{ todos.filter((item) => item.done !== true).length }} item left
      </p>
      <div class="middleButtons">
        <p @click="showAllTodos()" class="middleItem">All</p>
        <p @click="showActiveTodos()" class="middleItem">Active</p>
        <p @click="showCompletedTodos()" class="middleItem">Completed</p>
      </div>
      <p @click="deleteAllTodos()" class="rightItem">Clear All</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
let todoToAdd = ref("");
</script>

<script>
import Todo from "./Todo.vue";

export default {
  name: "Todos",
  data() {
    return {
      todos: [],
      //todosToShow: [],
    };
  },
  async mounted() {
    this.todos = await this.fetchTodos();
    this.todosToShow = await this.fetchTodos();
  },
  components: {
    Todo,
  },
  methods: {
    async fetchTodos() {
      const res = await fetch("api/todos");
      const data = await res.json();
      return data;
    },
    async fetchTodo(id) {
      const res = await fetch(`api/todos/${id}`);
      const data = await res.json();
      return data;
    },
    async addTodo(text) {
      if (text !== "") {
        const res = await fetch("/api/todos", {
          method: "POST",
          headers: {
            "Content-type": "application/json",
          },
          body: JSON.stringify({
            text: text,
            done: false,
          }),
        });
        const data = await res.json();

        this.todos.push(data);
        this.todoToAdd = "";
      } else {
        alert("Todo cannot be empty, please type something!");
      }
    },
    async toggleStatus(id) {
      const todoToToggle = await this.fetchTodo(id);
      const updatedTodo = { ...todoToToggle, done: !todoToToggle.done };

      const res = await fetch(`/api/todos/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updatedTodo),
      });
      const data = await res.json();
      this.todos = this.todos.map((todo) =>
        todo.id === id ? { ...todo, done: data.done } : todo
      );
    },
    toggleEdit(id) {
      console.log(id);
    },
    async toggleDelete(id) {
      const res = await fetch(`/api/todos/${id}`, {
        method: "DELETE",
      });
      res.status === 200
        ? (this.todos = this.todos.filter((todo) => todo.id !== id))
        : alert("Error deleting todo");
    },
    async deleteAllTodos() {
      console.log("Hello");
    },
    showAllTodos() {
      this.todos = this.todosToShow;
    },
    showActiveTodos() {
      this.todos = this.todosToShow.filter((todo) => todo.done !== true);
    },
    showCompletedTodos() {
      this.todos = this.todosToShow.filter((todo) => todo.done === true);
    },
  },
};
</script>

<style lang="scss" scoped>
.card {
  width: 50%;
  // height: 28em;
  // border: 2px solid gray;
  // border-radius: 0.5em;
  // z-index: 5;
}

.search {
  width: 100%;
  border: none;
  margin: 0.5em;
  padding: 15px 0px 15px 0px;
  font-style: italic;
  font-size: 20px;
}

.footer {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  margin: 0.5em;
  color: gray;
  font-size: 16px;
}

.leftItem {
  margin: 20px;
  font-weight: 500;
  color: red;
}

.middleButtons {
  display: flex;
  flex-direction: row;
}

.middleItem {
  margin: 10px;
  transition: all 0.2s ease-in-out;

  &:hover {
    cursor: pointer;
    transform: scale(1.1);
    color: red;
  }
}

.rightItem {
  margin: 20px;
  transition: all 0.2s ease-in-out;

  &:hover {
    cursor: pointer;
    transform: scale(1.1);
    color: red;
  }
}
</style>
