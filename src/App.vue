<script lang="ts">
import {defineComponent} from "vue";

import AppHeader from "./components/AppHeader.vue";
import AppFilters from "./components/AppFilters.vue";
import AppTodoList from "./components/AppTodoList.vue";
import AppAddTodo from "./components/AppAddTodo.vue";
import AppFooter from "./components/AppFooter.vue";
import {Todo} from "./types/Todo.ts";
import {Filter} from "./types/Filter.ts";
import {Stats} from "./types/Stats.ts";

interface State {
  todos: Todo[],
  activeFilter: Filter,
}

export default defineComponent({
  components: {
    AppFooter,
    AppAddTodo,
    AppTodoList,
    AppFilters,
    AppHeader,
  },
  data(): State {
    return {
      todos: [
        {
          id: 1,
          text: 'Learn the basics of Vue',
          completed: true
        },
        {
          id: 2,
          text: 'Learn the basics of Typescript',
          completed: false
        },
        {
          id: 3,
          text: 'Subscribe to the channel',
          completed: false
        },
      ],
      activeFilter: "All",
    }
  },
  computed: {
    filteredTodos(): Todo[] {
      switch (this.activeFilter) {
        case "Active":
          return this.activeTodos
        case "Done":
          return this.doneTodos
        case "All":
        default:
          return this.todos
      }
    },
    activeTodos(): Todo[] {
      return this.todos.filter((todo: Todo) => !todo.completed)
    },
    doneTodos(): Todo[] {
      return this.todos.filter((todo: Todo) => todo.completed)
    },
    stats(): Stats {
      return {
        active: this.activeTodos.length,
        done: this.doneTodos.length
      }
    }
  },
  methods: {
    toggleTodo(id: number) {
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id)

      if (targetTodo) {
        targetTodo.completed = !targetTodo.completed
      }
    },
    removeTodo(id: number) {
      this.todos = this.todos.filter((todo: Todo) => todo.id !== id)
    },
    addTodo(todo: Todo) {
      this.todos.push(todo)
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter
    }
  },
})
</script>
<template>
  <AppHeader/>

  <AppFilters :active-filter="activeFilter" @set-filter="setFilter"/>

  <main class="app-main">
    <AppTodoList :todos="filteredTodos" @toggleTodo="toggleTodo" @removeTodo="removeTodo"/>

    <AppAddTodo @add-todo="addTodo"/>
  </main>

  <AppFooter :stats="stats"/>
</template>
