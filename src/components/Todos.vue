<template>
  <div class="card">
    <input
      class="search"
      type="text"
      placeholder="     What needs to be done ?"
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
      <p class="leftItem">
        {{ todos.filter((item) => item.done !== true).length }} items left
      </p>
      <div class="middleButtons">
        <p @click="showAllTodos()" class="middleItem">All</p>
        <p @click="showActiveTodos()" class="middleItem">Active</p>
        <p @click="showCompletedTodos()" class="middleItem">Completed</p>
      </div>
      <p @click="deleteAllTodos()" class="rightItem">Clear completed</p>
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
      todosToShow: [],
    };
  },
  created() {
    if (JSON.parse(localStorage.getItem("allTodos")) !== null) {
      this.todos = JSON.parse(localStorage.getItem("allTodos"));
    } else {
      this.todos = [];
    }
    localStorage.setItem("allTodos", JSON.stringify(this.todos));
    this.todosToShow = this.todos;
  },
  components: {
    Todo,
  },
  methods: {
    addTodo(text) {
      if (text !== "") {
        this.todos.push({
          id: Math.floor(Math.random() * 100),
          text: text,
          done: false,
        });
        this.todoToAdd = "";
        localStorage.setItem("allTodos", JSON.stringify(this.todos));
      } else {
        alert("Todo cannot be empty, please type something!");
      }
    },
    toggleStatus(id) {
      let todo = this.todos.filter((todo) => todo.id === id)[0];
      todo.done = !todo.done;
      localStorage.setItem("allTodos", JSON.stringify(this.todos));
      // if (this.todos[id].done === true) {
      //   this.todos[id].done = false;
      // } else if (this.todos[id].done === false) {
      //   this.todos[id].done = true;
      // }
    },
    toggleEdit(id) {
      console.log(id);
    },
    toggleDelete(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
      localStorage.setItem("allTodos", JSON.stringify(this.todos));
    },
    deleteAllTodos() {
      this.todos = [];
      localStorage.removeItem("allTodos");
    },
    showAllTodos() {
      this.todos = this.todosToShow;
      localStorage.setItem("allTodos", this.todosToShow);
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
  border: 2px solid gray;
}

.search {
  width: 100%;
  border: none;
  padding: 15px 0px 15px 0px;
  font-style: italic;
  font-size: 20px;
}

.footer {
  display: flex;
  flex-direction: row;
  height: 50px;
  justify-content: space-between;
  align-items: center;
  border-top: 1.5px solid gray;
  color: gray;
  font-size: 16px;
}

.leftItem {
  margin: 10px;
}

.middleButtons {
  display: flex;
  flex-direction: row;
}

.middleItem {
  margin: 10px;

  &:hover {
    cursor: pointer;
  }
}

.rightItem {
  margin: 10px;

  &:hover {
    cursor: pointer;
  }
}
</style>
