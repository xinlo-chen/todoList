<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <span class="title">
          <h2>{{ title }}</h2>
        </span>
        <my-header @addTodo="addTodo" />
        <my-list :todos="todos" />
        <my-footer :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo" />
      </div>
    </div>
  </div>
</template>

<script>
import PubSub from 'pubsub-js'
import MyFooter from './components/MyFooter.vue'
import MyHeader from './components/MyHeader.vue'
import MyList from './components/MyList.vue'

export default {
  components: { MyHeader, MyList, MyFooter },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem('todos')) || [
        { id: '001', title: '学习新知识', done: false, date: '08-29' },
        { id: '002', title: '锻炼1h', done: false, date: '08-30' },
      ],
      title: '随 手 记',
    }
  },

  watch: {
    todos: {
      deep: true,
      handler() {
        localStorage.setItem('todos', JSON.stringify(this.todos))
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
    deleteTodo(_, id) {
      this.todos = this.todos.filter((todo) => todo.id !== id)
      console.log(id)
    },
    checkAllTodo(value) {
      this.todos.forEach((todo) => {
        todo.done = value
      })
    },
    clearAllTodo(done) {
      this.todos = this.todos.filter((todo) => (todo.done = done))
    },
    updateTodo(id, value) {
      this.todos.forEach((todo) => {
        if (todo.id === id) todo.title = value
      })
    },
  },
  mounted() {
    this.$bus.$on('checkTodo', this.checkTodo)
    this.$bus.$on('updateTodo', this.updateTodo)
    this.pubId = PubSub.subscribe('deleteTodo', this.deleteTodo)
  },
  beforeDestroy() {
    this.$bus.$off('checkTodo')
    this.$bus.$off('updateTodo')
    PubSub.unsubscribe(this.pubId)
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
.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid rgb(103, 159, 180);
  margin-right: 5px;
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
