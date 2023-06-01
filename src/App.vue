<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')
const input_content = ref('')
const input_category = ref(null)

const asc_todos = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

const addTodo = () => {
  if(input_content.value === '' || input_category.value === null) return
  // console.log('add todo!');
  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })
  console.log(todos);
}
const removeTodo = (todo) => {
  todos.value = todos.value.filter((filTodo) => filTodo !== todo)
}

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })


onMounted(() => {
  // console.log(localStorage);
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>

<template>
  <main class="container">
    <div class="sketchy">

      <section class="greeting">
        <h2 class="title">
          What's up, <input type="text" class="box" placeholder="Name here" v-model="name" />
        </h2>
      </section>
      <section class="create-todo">
        <h3>Create a Todo</h3>
        <form @submit.prevent="addTodo">
          <h4>Your todo list :</h4>
          <input type="text" class="box w-100" placeholder="exemple: Save the world!" v-model="input_content" />
          <h4>Pick a category</h4>
          <div class="options">
            <label>
              <input type="radio" class="bubble-professional" name="category" value="professional" v-model="input_category">
              <span>Professional</span>
            </label>
            <label>
              <input type="radio" class="bubble-personal" name="category" value="personal" v-model="input_category">
              <span>Personal</span>
            </label>
  
          </div>
          <div class="button">
            <input type="submit" class="button-paper" value="Add todo">
          </div>
        </form>
      </section>
      <section class="todo-list">
        <h3>TodoList</h3>
        <div class="list">
          <div v-for="todo in asc_todos" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done"/>
            <span :class="`bubble-${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" class="box bottom" v-model="todo.content"/>
          </div>
          <div class="actions">
            <button class="button-paper delete" @click="removeTodo(todo)">Delete</button>
          </div>
        
        </div>
        </div>
      </section>
      {{  asc_todos }}
    </div>
  </main>
</template>

<style>


</style>
