<template>
  <ul class="to-do-list-main">
    <!-- 不仅需要根据对象数组的长度生成相同数量的TodoItem组件，还需要将数据传给TodoItem组件 -->
    <TodoItem
      v-for="todoObj in todoList"
      :key="todoObj.id"
      :todo="todoObj"
    ></TodoItem>
  </ul>
</template>

<script>
import TodoItem from "./TodoItem.vue";

export default {
  name: "TodoList",
  components: {
    TodoItem,
  },
  props: ["todoList"],
};
</script>

<style scoped>
/* TodoList的样式 */
.to-do-list-main {
  border: 1px solid #ddd;
  border-radius: 3px;
  min-height: 32vh;
}

.to-do-list-empty {
  height: 32px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 3px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>
