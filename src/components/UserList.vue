<template>
  <ul class="todo-main">
    <UserItem
      v-for="item in todos" :key="item.id"
      :todo="item" />
  </ul>
</template>

<script>
import UserItem from './UserItem'

export default {
  name: 'UserList',
  components: { UserItem },
  props: ['todos']
}
</script>

<style scoped>
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>
