<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <UserHeader
          @addTodo="addTodo" />
        <UserList
          :todos="todos" />
        <UserFooter
          :todos="todos"
          @checkAllTodo="checkAllTodo"
          @clearAllTodo="clearAllTodo" />
      </div>
    </div>
  </div>
</template>

<script>
import UserHeader from './components/UserHeader'
import UserList from './components/UserList'
import UserFooter from './components/UserFooter'

export default {
  name: 'App',
  components: { UserHeader, UserList, UserFooter },
  data() {
    return {
      todos: JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  watch: {
    todos: {
      deep: true,
      handler(value) {
        localStorage.setItem('todos', JSON.stringify(value))
      }
    }
  },
  methods: {
    // 添加一个 todo
    addTodo(todo) {
      this.todos.unshift(todo)
    },
    // 勾选 & 取消勾选一个 todo
    checkTodo(id) {
      this.todos.forEach(todo => {
        if (todo.id === id) {
          todo.done = !todo.done
        }
      })
    },
    // 删除一个 todo
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id)
    },
    // 全选 & 取消全选所有 todo
    checkAllTodo(done) {
      this.todos.forEach(todo => {
        todo.done = done
      })
    },
    // 清除已完成的 todo
    clearAllTodo() {
      this.todos = this.todos.filter(todo => !todo.done)
    },
    // 更新一个 todo
    updateTodo(id, title) {
      this.todos.forEach(todo => {
        if (todo.id === id) {
          todo.title = title
        }
      })
    }
  },
  mounted() {
    this.$bus.$on('checkTodo', this.checkTodo)
    this.$bus.$on('deleteTodo', this.deleteTodo)
    this.$bus.$on('updateTodo', this.updateTodo)
  },
  beforeDestroy() {
    this.$bus.$off(['checkTodo', 'deleteTodo', 'updateTodo'])
  }
}
</script>

<style>
body {
  background: #fff;
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
  background-color: #49a2da;
  border: 1px solid #207cb5;
  margin-right: 5px;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn-edit:hover {
  color: #fff;
  background-color: #207cb5;
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
