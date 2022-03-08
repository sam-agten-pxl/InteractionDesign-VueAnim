<template>
  <div class="home">
    <transition name="toast">
        <Toast v-if="showToast" />
    </transition>
    <div class="todos">
        <input 
            type="text" 
            v-model="newTodo" 
            @keypress.enter="addTodo(newTodo)"
            placeholder="Add a new todo..."
        />
        <div v-if="todos.length">
            <transition-group tag="ul" name="list" appear>
            <li v-for="todo in todos" :key="todo.id" @click="this.deleteTodo(todo.id)">
                {{ todo.text }}
            </li>
            </transition-group>
        </div>
        <div v-else>Woohoo, nothing left todo!</div>
        </div>
    </div>
</template>

<script>
import Toast from '../components/Toast'

export default {
  components: { Toast },
  data: function() {
      return {
          showToast: false,
          todos: [
              {text: 'Make planning', id: 1},
              {text: 'Play Midnight Protocol', id: 2}
          ]
      }
  },
  mounted() {

  },
  methods: {
      triggerToast: function() {
          this.showToast = true;
          setTimeout(() => this.showToast = false, 3000);
      },
      addTodo: function(newTodo) {
          if(newTodo)
          {
              const id = Math.random();
              this.todos = [{text: newTodo, id}, ...this.todos]
          }
          else
          {
              this.triggerToast();
          }
      },
      deleteTodo: function(id) {
          this.todos = this.todos.filter(todo => todo.id != id);
      }
  }
}
</script>

<style>
    .todos {
        max-width: 400px;
        margin: 20px auto;
        position: relative;
    }
    input {
        width: 100%;
        padding: 12px;
        border: 1px solid #eee;
        border-radius: 10px;
        box-sizing: border-box;
        margin-bottom: 20px;
    }
    .todos ul {
        position: relative;
        padding: 0;
    }
    .todos li {
        list-style-type: none;
        display: block;
        margin-bottom: 10px;
        padding: 10px;
        background: white;
        box-shadow: 1px 3px 5px rgba(0,0,0,0.1);
        border-radius: 10px;
        width: 100%;
        box-sizing: border-box;
    }
    .todos li:hover {
        cursor: pointer;
    }

    .toast-enter-active {
        animation: wobble 0.5s ease;
    }

    .toast-leave-from {
        opacity: 1;
    }
    .toast-leave-active {
        transition: all 0.2s ease;
    }
    .toast-leave-to {
        opacity: 0;
        transform: translateY(-60px);
    }

    @keyframes wobble
    {
        0% {transform: translateY(-60px); opacity: 0;}
        50% {transform: translateY(0); opacity: 1;}
        60% {transform: translateX(8px);}
        70% {transform: translateX(-8px);}
        80% {transform: translateX(4px);}
        90% {transform: translateX(-4px);}
        100% {transform: translateX(0);}
        
    }

    .list-enter-from {
        opacity: 0;
        transform: scale(0.6);
    }
    .list-enter-to {
        opacity: 1;
        transform: scale(1);
    }
    .list-enter-active {
        transition: all 0.2s ease;
    }

    .list-leave-from {
        opacity: 1;
        transform: scale(1);
    }

    .list-leave-active {
        transition: all 0.2s ease;
        position: absolute;
    }

    .list-leave-to {
        opacity: 0;
        transform: scale(0.6);
    }

    .list-move {
        transition: all 0.2s ease;
    }
  
</style>