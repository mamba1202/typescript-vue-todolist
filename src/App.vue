<template>
  <div id="app">
    <div class="inner">
      <NewTodo @addTodo="addTodo"></NewTodo>
      <TodoList
        :list="list"
        @updateTodo="updateTodo"
      ></TodoList>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import NewTodo from "./components/NewTodo.vue";
import TodoList from "./components/TodoList.vue";
import Todo from "./models/Todo";

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
  updateTodo(todo: Todo, part: Partial<Todo>) {
    let index: number = this.list.indexOf(todo); //找到改变的
    let newTodo: Todo = Object.assign({}, todo, part); //新的数据
    this.list.splice(index, 1, newTodo); //将新的数据放入
  }
}
</script>

<style scoped lang="scss">
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

<style lang="scss">
* {
  margin: 0;
  padding: 0;
}
ul,
ol {
  list-style: none;
}
</style>
