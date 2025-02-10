<template>
  <div class="todo">
    <h1 class="todo__title">{{ title }}</h1>
    
    <form @submit.prevent="handleSubmit" class="todo__form">
      <input 
        v-model="newTodo" 
        type="text" 
        placeholder="Tilføj ny opgave"
        class="todo__form-input"
        :class="{ 'todo__form-input--editing': isEditing }"
      >
      <button type="submit" class="todo__form-button">
        {{ isEditing ? 'Opdater' : 'Tilføj' }}
      </button>
    </form>

    <ul class="todo__list" v-if="todos.length">
      <li 
        v-for="todo in todos" 
        :key="todo.id" 
        class="todo__item"
      >
        <div class="todo__item-content">
          <div class="todo__item-text">{{ todo.text }}</div>
        </div>
        <div class="todo__item-actions">
          <button 
            @click="editTodo(todo)"
            class="todo__item-button todo__item-button--edit"
          >
            Rediger
          </button>
          <button 
            @click="deleteTodo(todo.id)"
            class="todo__item-button todo__item-button--delete"
          >
            Slet
          </button>
        </div>
      </li>
    </ul>
    
    <p v-else class="todo__empty">Ingen opgaver endnu!</p>
  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  data() {
    return {
      title: 'Min Todo Liste',
      todos: [],
      newTodo: '',
      isEditing: false,
      editingId: null
    }
  },
  methods: {
    handleSubmit() {
      if (!this.newTodo.trim()) return
      if (this.isEditing) {
        this.updateTodo()
      } else {
        this.addTodo()
      }
    },

    addTodo() {
      this.todos.push({
        id: Date.now(),
        text: this.newTodo
      })
      this.newTodo = ''
    },

    updateTodo() {
      const index = this.todos.findIndex(todo => todo.id === this.editingId)
      if (index !== -1) {
        this.todos[index].text = this.newTodo
      }
      this.newTodo = ''
      this.isEditing = false
      this.editingId = null
    },

    editTodo(todo) {
      this.newTodo = todo.text
      this.isEditing = true
      this.editingId = todo.id
    },

    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id)
    }
  }
}
</script>
