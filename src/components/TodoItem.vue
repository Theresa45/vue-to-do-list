<template>
  <transition
    appear
    name="animate__animated animate__bounce"
    enter-active-class="animate__fadeIn"
    leave-active-class="animate__fadeOut"
  >
    <li>
      <label>
        <input
          type="checkbox"
          :checked="todo.isCompleted"
          @change="handleCheckbox(todo.id)"
        />
        <span v-show="!todo.isEdit">{{ todo.description }}</span>
        <input
          type="text"
          class="edit-description"
          v-show="todo.isEdit"
          v-model="updatedDescription"
          ref="content"
          @blur="handleBlur(todo)"
          @keyup.enter="handleTriggerBlur"
        />
      </label>
      <div>
        <button
          class="btn btn-edit"
          v-show="!todo.isEdit"
          @click="handleClickEdit(todo)"
        >
          编辑
        </button>
        <button class="btn btn-delete" @click="handleDelete(todo.id)">
          删除
        </button>
      </div>
    </li>
  </transition>
</template>

<script>
import "animate.css";
export default {
  name: "TodoItem",
  props: ["todo"],
  data() {
    return {
      updatedDescription: this.todo.description,
    };
  },
  methods: {
    // 触发checkTodo事件
    handleCheckbox(id) {
      this.$bus.$emit("checkTodo", id);
    },
    // 触发deleteTodo事件
    handleDelete(id) {
      if (confirm("确定删除任务吗？🤔")) this.$bus.$emit("deleteTodo", id);
    },
    // 触发editTodo事件
    handleClickEdit(todo) {
      // this.$set()用于添加一个响应式属性，只需要执行一次即可
      if (todo.hasOwnProperty.call(todo, "isEdit")) todo.isEdit = true;
      else this.$set(todo, "isEdit", true);
      this.$nextTick(() => this.$refs.content.focus());
    },
    // 失去焦点，保存更新
    handleBlur(todo) {
      // 此时，todo对象上已经有isEdit属性了
      todo.isEdit = false;
      if (!this.updatedDescription.trim()) return alert("输入不能为空！");
      this.$bus.$emit("editTodo", todo.id, this.updatedDescription);
    },
    // 按下Enter键，保存更新
    handleTriggerBlur() {
      this.$nextTick(() => this.$refs.content.blur());
    },
  },
};
</script>

<style scoped>
/* TodoItem的样式 */

li {
  list-style: none;
  height: 32px;
  line-height: 32px;
  padding: 0 8px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li div {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
}

input {
  vertical-align: middle;
  margin-right: 8px;
  position: relative;
  top: -1px;
}
.edit-description {
  line-height: 32px;
  outline: none;
  border: none;
  font-size: 16px;
  background-color: transparent;
}

li button {
  display: none;
  margin-top: 3px;
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
</style>
