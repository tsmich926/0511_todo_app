<template>
  <div>
    <ul>
      <li v-for="todo in todos" 
      :key="todo.id">
          <span 
          @click="updateTodoStatus(todo)" 
          :class="{'is-completed': todo.is_completed }"
          > 
        {{ todo.title }}
        </span> 
        <button @click="deleteTodo(todo)" class="todo-btn">X</button>
      </li> 
    </ul>
    <button @click="getTodos">Get Todos</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'TodoList',
  data: function () {
    return {
      todos: [{title:'장고',is_completed:true}],
    }
  },

    created(){
    this.getTodos()
  },
  methods: {
    getTodos: function () {
      axios({
        method: 'get',
        url: 'http://127.0.0.1:8000/todos/',
      })
        .then(res => {
          console.log(res)
          this.todos = res.data
  
        })
        .catch(err => {
          console.log(err)
        })
    },

    deleteTodo: function (todo) {
      axios({
        method: 'DELETE',
        url: `http://127.0.0.1:8000/todos/${todo.id}/`,
      })      
        .then(res=>{
          console.log(res)
          this.getTodos()
        })     
      }
    },

    updateTodoStatus: function (todo) {
      axios({
        method:'PUT',
        url: `http://127.0.0.1:8000/todos/${todo.id}/`,
        data: {is_completed:!todo.is_completed, title:todo.title}
      })
        .then(res=>{
          console.log(res)
          this.getTodos()
      })
    },
  }

</script>

<style scoped>
  .todo-btn {
    margin-left: 10px;
  }

  .is-completed {
    text-decoration: line-through;
    color: rgb(112, 112, 112);
  }
</style>
