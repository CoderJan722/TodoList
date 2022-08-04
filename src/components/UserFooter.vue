<template>
  <div class="todo-footer"
    v-show="total">
    <label>
      <input type="checkbox"
        v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
    </span>
    <button class="btn btn-danger"
      @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: 'UserFooter',
  props: ['todos'],
  methods: {
    // 清除已完成的 todo
    clearAll() {
      this.$emit('clearAllTodo')
    }
  },
  computed: {
    // 统计 todo
    total() {
      return this.todos.length
    },
    // 统计已完成的 todo
    doneTotal() {
      // 遍历 todos, todo.done === true 时计数器 + 1
      return this.todos.reduce((pre, todo) => pre + (todo.done ? 1 : 0), 0)
    },
    // 是否全部勾选
    isAll: {
      get() {
        return this.doneTotal === this.total && this.total > 0
      },
      set(done) {
        this.$emit('checkAllTodo', done)
      }
    }
  }
}
</script>

<style scoped>
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
