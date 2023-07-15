<script lang="ts">
import {defineComponent} from 'vue'
import {Todo} from "@/types/Todo";


interface State {
  isFormVisible: boolean,
  todoText: string,
}

export default defineComponent({
  data(): State {
    return {
      isFormVisible: false,
      todoText: '',
    }
  },
  methods: {
    toggleFormVisible() {
      this.isFormVisible = !this.isFormVisible
    },
    addTodo() {
      this.$emit('addTodo', {
        id: Date.now(),
        text: this.todoText,
        completed: false,
      })
      this.todoText = ''
    }
  },
  emits: {
    addTodo: (todo: Todo) => todo
  }
})
</script>

<template>
  <section class="add-todo">
    <form v-if="isFormVisible" class="add-todo__form" @submit.prevent="addTodo">
      <button class="close-button" type="button" @click="toggleFormVisible">
        <i class="bi bi-x"></i>
      </button>
      <div class="text-input text-input--focus">
        <input class="input" v-model="todoText"/>
      </div>
      <button class="button button--filled">Add task</button>
    </form>
    <button v-else class="add-todo__show-form-button" @click="toggleFormVisible">
      <i class="bi bi-plus-lg"></i>
    </button>
  </section>
</template>