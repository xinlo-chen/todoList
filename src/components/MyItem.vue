<template>
  <li>
    <label>
      <input type="checkbox" :checked="todo.done" @change="handelCheck" />
      <span>{{ todo.title }}</span>
    </label>
    <span class="box">
      <span class="note1" v-text="todo.notes"></span>
      <input class="note" type="text" placeholder="点击添加或修改备注..." v-model.lazy="notes" @click="update" />
    </span>
    <button class="btn btn-danger" @click="handelDelete">删除</button>
    <span class="date">{{ todo.date }}</span>
  </li>
</template>

<script>
export default {
  name: 'MyItem',
  props: ['todo', 'checkTodo', 'deleteTodo', 'updateNotes'],
  data() {
    return {
      notes: '',
    }
  },
  methods: {
    handelCheck() {
      this.checkTodo(this.todo.id)
    },
    handelDelete() {
      if (confirm('请确认是否删除该项')) this.deleteTodo(this.todo.id)
    },
    update() {
      console.log(this.notes)
    },
  },
  watch: {
    notes(value) {
      this.updateNotes(this.todo.id, value)
    },
  },
}
</script>

<style scoped>
/*item*/
li {
  position: relative;
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}
li .box {
  float: left;
}
li label span {
  font-size: 17px;
  margin-left: 5px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}
li .date {
  position: absolute;
  right: 5px;
  line-height: 36px;
  width: 60px;
  text-align: center;
  font-size: 12px;
}
li:hover .date {
  display: none;
}
li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}

li:hover button {
  display: block;
}
li .note,
li .note1 {
  position: absolute;
  width: 300px;
  font-size: 12px;
  border: none;
  margin-left: 10px;
  line-height: 37px;
  background: rgba(0, 0, 0, 0);
}
li .note:focus {
  outline: none;
}
li .note {
  display: none;
}
li:hover .note {
  display: block;
}
li .note1 {
  display: block;
}
li:hover .note1 {
  display: none;
}
</style>
