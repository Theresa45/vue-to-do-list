<template>
  <div class="to-do-list-container">
    <div class="to-do-list-intro">
      <img src="./assets/logo.png" alt="logo" />
      <h4 class="to-do-list-title">To-Do-List</h4>
    </div>
    <div class="to-do-list-wrap">
      <!-- 父组件给子组件传递函数 -->
      <TodoHeader @addTodo="addTodo"></TodoHeader>
      <!-- 父组件给子组件传递数据 -->
      <TodoList :todoList="todoList"></TodoList>
      <TodoFooter
        :todoList="todoList"
        @clearCompleted="clearCompleted"
        @checkAll="checkAll"
      ></TodoFooter>
    </div>
  </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader.vue";
import TodoList from "./components/TodoList.vue";
import TodoFooter from "./components/TodoFooter.vue";
//因为TodoItem是TodoList的子组件，所以只需要引入TodoList即可
// import TodoItem from "./components/TodoItem.vue";

export default {
  name: "App",
  components: {
    TodoHeader,
    TodoList,
    TodoFooter,
  },
  data() {
    return {
      // 因为需要存储在浏览器本地中，所以要将对象转换成JSON。
      todoList: JSON.parse(localStorage.getItem("todoList")) || [],
    };
  },
  watch: {
    // 实现浏览器的本地存储，每一次数据发生变化都要重新存储
    todoList: {
      handler(newValue) {
        localStorage.setItem("todoList", JSON.stringify(newValue));
      },
      deep: true,
    },
  },
  methods: {
    // 添加一个任务
    addTodo(todoObj) {
      this.todoList.unshift(todoObj);
    },
    // 勾选或取消勾选一个任务
    checkTodo(id) {
      this.todoList.forEach((todo) => {
        if (todo.id === id) todo.isCompleted = !todo.isCompleted;
      });
    },
    // 删除一个任务
    deleteTodo(id) {
      this.todoList = this.todoList.filter((todo) => todo.id !== id);
    },
    // 全选或取消全选
    checkAll(isCompleted) {
      this.todoList.forEach((todo) => (todo.isCompleted = isCompleted));
    },
    // 清除所有已完成的任务
    clearCompleted() {
      this.todoList = this.todoList.filter((todo) => !todo.isCompleted);
    },
    // 编辑一个任务
    editTodo(id, updatedDescription) {
      this.todoList.forEach((todo) => {
        if (todo.id === id) todo.description = updatedDescription;
      });
    },
  },
  // 全局事件总线的使用方式
  mounted() {
    this.$bus.$on("checkTodo", this.checkTodo);
    this.$bus.$on("deleteTodo", this.deleteTodo);
    this.$bus.$on("editTodo", this.editTodo);
  },
  beforeDestroy() {
    this.$bus.$off(["checkTodo", "deleteTodo", "editTodo"]);
  },
};
</script>

<style>
/* 基础样式——所有组件都会应用的样式 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #f1f2f5;
}

img {
  width: 16px;
  height: 16px;
  margin-right: 8px;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 16px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 3px;
}

.btn:hover {
  transform: scale(1.05);
}

.btn:focus {
  outline: none;
}

.btn-delete {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-delete:hover {
  background-color: #bd362f;
}

.btn-edit {
  color: #fff;
  background-color: rgba(82, 168, 236, 0.6);
  border: 1px solid rgba(82, 168, 236);
}

.btn-edit:hover {
  background-color: rgba(82, 168, 236);
}

.to-do-list-container {
  min-height: 60vh;
  margin: 10vh 32px;
  border-radius: 3px;
  background-color: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.25);
}

.to-do-list-intro {
  height: 50px;
  padding: 0 16px;
  display: flex;
  align-items: center;
}

.to-do-list-title {
  font-size: 16px;
  font-weight: 600;
  opacity: 0.9;
  color: #545862;
}

.to-do-list-wrap {
  padding: 0 16px;
  border-radius: 3px;
  /* border: 1px solid #ddd; */
}

/* 媒体查询：视口宽度大于768px时，容器的样式 */
@media (min-width: 768px) {
  .to-do-list-container {
    width: 600px;
    margin: 10vh auto;
  }
}
</style>
