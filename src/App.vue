<script setup>


import { computed, onMounted, ref, watch } from "vue";
const todo = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)
const todo_list = computed(()=>todo.value.sort((a, b)  => {
  return b.createdAt - a.createdAt


}))

const addTodo = () => {
  if(input_content.value.trim( ) === '' || input_category.value === null){
    return

  }
  todo.value.push({
    content:input_content.value,
    category: input_category.value,
    done:false,
    createdAt: new Date().getTime()
  })

  input_content.value = ''
  input_category.value = null
  name.value= ''
 


}
const removeTodo = todos =>{
  todo.value = todo.value.filter(t=>t!==todos )
}
watch(todo, newValue =>{
  localStorage.setItem('todo', JSON.stringify(newValue))
}, {deep: true})


const inputRef = ref(null)
onMounted(()=>{
  inputRef.value.focus()
})
watch(name, (newValue) =>{
  localStorage.setItem('name' , newValue)
})

onMounted(()=>{
  name.value = localStorage.getItem('name') || ''
  todo.value = JSON.parse(localStorage.getItem('todo') || [])
})
 

</script>



<template>

  <main class="app">

    <section class="greeting">
      <h2 class="user-greet">welcome, <input type="text" 
        id="name-input"
         placeholder="Name here" 
         v-model="name"
          ref="inputRef"/></h2>
    </section>

    <section class="create_todo">
      <h3>Create a Todo</h3>

      <form action="" @submit.prevent="addTodo">
        <h4>What do you intend to acheive this week🤗</h4>
        <input type="text"
         placeholder="e.g Go to the gym"
         class="input"
         v-model="input_content"/>
        <h4>Pick a category</h4>
        <div class="options">
          <label for="" class="bubble">
            <input 
            type="radio"
             name="category" 
             id="category1"
             value="Important"
             class="inportant"
             v-model="input_category">
             <span class="buble-important"></span>
             <div> Important </div>
          </label>

          <label for="" class="bubble">
            <input 
            type="radio"
             name="category" 
             id="category2"
             value="Trivial"
             class="trivial"
             v-model="input_category">
             <span class="bubble-Trivial"></span>
             <div> Trivial </div>
           
          </label>

            
        </div>

        <input type="submit"
         value="Add Todo" 
         id="submit" class="todo-button">


      

      </form>
 
    </section>

    <section class="todo-list ">
      <h3>Todo List</h3>
      <div 
      v-for="todos in todo_list" 
      class="todo-items">
        <label for="" class="">
          <input
           type="checkbox" 
           v-model="todos.done"
            id="">
            <span></span>
        </label>
        <div class="todo-content ">
          <input type="text" 
          id="" 
          v-model="todos.content">

        </div>

        <div class="actions">
          <button class="delete" 
          @click="removeTodo(todos)">Delete</button>
        </div>
      

      </div>
    </section>

   
  </main>
  
</template>

<style>


</style>