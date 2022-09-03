<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @change="handelCheck(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input
        type="text"
        v-show="todo.isEdit"
        :value="todo.title"
        @blur="handleBlur(todo, $event)"
        ref="inputTitle"
      />
    </label>
    <!-- <span class="box">
      <span class="note1" v-text="todo.notes" v-show="!todo.isEdit"></span>
      <input class="note" type="text" placeholder="点击添加或修改备注..." v-model="notes" v-show="todo.isEdit" @blur="update(todo)" />
    </span> -->
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
    <button
      v-show="!todo.isEdit"
      class="btn btn-edit"
      @click="handleEdit(todo)"
    >
      编辑
    </button>
    <span class="date">{{ todo.date }}</span>
  </li>
</template>

<script>
import PubSub from "pubsub-js";
export default {
  name: "MyItem",
  props: ["todo"],
  methods: {
    handelCheck(id) {
      this.$bus.$emit("checkTodo", id);
    },
    handleDelete(id) {
      if (confirm("请确认是否删除该项")) PubSub.publish("deleteTodo", id);
    },
    handleEdit(todo) {
      // eslint-disable-next-line no-prototype-builtins
      if (todo.hasOwnProperty("isEdit")) {
        todo.isEdit = true;
      } else {
        // console.log('@')
        this.$set(todo, "isEdit", true);
      }
      this.$nextTick(function () {
        this.$refs.inputTitle.focus();
      });
    },
    handleBlur(todo, e) {
      todo.isEdit = false;
      if (!e.target.value.trim()) return alert("输入不能为空！");
      this.$bus.$emit("updateTodo", todo.id, e.target.value);
    },

    /*   update(id, notes) {
      this.$bus.$emit('updateNotes', id, notes)
      console.log(id, notes)
    }, */
  },
  /* watch: {
    notes(value) {
      this.$bus.$emit('updateNotes', this.todo.id, value)
    },
  }, */
};
</script>

<style scoped>
/*item*/
li {
  position: relative;
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 1px 5px;
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
li .note1 {
  display: block;
  color: rgb(16, 163, 248);
}
</style>
