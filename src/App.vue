<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <span class="title">
          <h2>{{ title }}</h2>
        </span>
        <my-header :addTodo="addTodo" />
        <my-list :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo" :updateNotes="updateNotes" />
        <my-footer :todos="todos" :checkAllTodo="checkAllTodo" :clearAllTodo="clearAllTodo" />
      </div>
    </div>
  </div>
</template>

<script>
import MyFooter from './components/MyFooter.vue'
import MyHeader from './components/MyHeader.vue'
import MyList from './components/MyList.vue'

export default {
  components: { MyHeader, MyList, MyFooter },
  data() {
    return {
      todos: [
        { id: '001', title: '背面试题', done: true, notes: '', date: '08-30' },
        { id: '002', title: '学习Vue', done: false, notes: '', date: '08-29' },
        { id: '003', title: '复习JS知识点', done: true, notes: '', date: '08-28' },
      ],
      title: '随 手 记',
    }
  },
  mounted() {
    this.updateTodos()
    console.log('替换执行一次')
  },
  watch: {
    todos: {
      deep: true,
      handler() {
        localStorage.setItem('todos', JSON.stringify(this.todos))
        console.log('存储了一次')
      },
    },
  },

  methods: {
    addTodo(todoObj) {
      this.todos.unshift(todoObj)
    },
    checkTodo(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) todo.done = !todo.done
      })
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id)
    },
    checkAllTodo(value) {
      this.todos.forEach((todo) => {
        todo.done = value
      })
    },
    clearAllTodo() {
      this.todos = this.todos.filter((todo) => !todo.done)
    },
    updateNotes(id, value) {
      this.todos.forEach((todo) => {
        if (todo.id === id) todo.notes = value
      })
    },
    updateTodos() {
      const todos = JSON.parse(localStorage.getItem('todos'))
      this.todos = todos
    },
  },
}
</script>

<style>
/*base*/
body {
  background: #fff;
}
h2 {
  margin: 0;
  padding: 5px 0 10px;
}
.title {
  text-align: center;
  padding: 0px 0;
}
.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}
.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}
.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}
.btn:focus {
  outline: none;
}
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
