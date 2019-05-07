<template>
  <div id="app">
    <div class="inner">
      <NewTodo @addTodo="addTodo"></NewTodo>
      <TodoList :list="list"></TodoList>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import NewTodo from "./components/NewTodo.vue";
import TodoList from "./components/TodoList.vue";

interface Todo {
  name: String;
  status: "done" | "todo" | "deleted";
}

@Component({
  components: {
    NewTodo,
    TodoList
  },
  watch: {
    list(newValue: Array<Todo>) {
      let string = JSON.stringify(newValue);
      localStorage.setItem("data", string);
    }
  }
})
export default class App extends Vue {
  list: Array<Todo> = localStorage.getItem("data")
    ? JSON.parse(<string>localStorage.getItem("data"))
    : [];
  addTodo(name: String) {
    let todo: Todo = { name: name, status: "todo" }; //注意声明类型
    this.list.push(todo);
  }
}
</script>

<style lang="scss">
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  > .inner {
    border: 1px solid grey;
    padding: 20px;
  }
}
</style>
