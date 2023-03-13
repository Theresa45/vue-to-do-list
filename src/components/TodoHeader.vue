<template>
  <div class="to-do-list-header">
    <!-- 绑定键盘事件，指定Enter键 -->
    <input
      type="text"
      placeholder="请输入你的任务，按 Enter 确认"
      v-model="description"
      @keyup.enter="add"
    />
  </div>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  name: "TodoHeader",
  data() {
    return {
      description: "",
    };
  },
  methods: {
    add() {
      // 校验数据
      if (!this.description.trim()) return alert("输入不能为空！");
      const todoObj = {
        id: nanoid(),
        description: this.description,
        isCompleted: false,
      };
      // 通知App组件添加一个todo对象
      this.$emit("addTodo", todoObj);
      // 清空输入字段
      this.description = "";
    },
  },
};
</script>

<style scoped>
/* TodoHeader的样式 */
.to-do-list-header {
  margin-bottom: 16px;
}
.to-do-list-header input {
  width: 100%;
  height: 32px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 3px;
  padding: 4px 8px;
}

.to-do-list-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>
