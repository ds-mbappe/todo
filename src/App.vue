<template>
  <h1 class="header">Todos</h1>
  <Todos />
</template>

<script>
import Todos from "./components/Todos.vue";

export default {
  name: "App",
  components: {
    Todos,
  },
};
</script>

<style lang="scss">
#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 5em;
  margin-left: 2em;
  margin-right: 2em;
  border: 0.15em solid gray;
  border-radius: 1em;
}

.header {
  margin-top: 1em;
  margin-bottom: 2em;
}
</style>
