<template>
  <!-- 当没有任务列表的时候，该Footer可以不展示 -->
  <div class="to-do-list-footer" v-show="total">
    <label>
      <input type="checkbox" v-model="isAll" />
      <span>全选</span>
    </label>
    <span>已完成 {{ completedTotal }} / {{ total }}</span>
    <button class="btn btn-delete" @click="handleClearCompleted">
      清除所有已完成的任务
    </button>
  </div>
</template>

<script>
export default {
  name: "TodoFooter",
  props: ["todoList"],
  computed: {
    total() {
      return this.todoList.length;
    },
    completedTotal() {
      return this.todoList.filter((todo) => todo.isCompleted).length;
    },
    isAll: {
      get() {
        return this.completedTotal === this.total && this.total > 0;
      },
      set(value) {
        this.$emit("checkAll", value);
      },
    },
  },
  methods: {
    handleClearCompleted() {
      if (confirm("确定清除所有已完成的任务吗？ 🤔"))
        this.$emit("clearCompleted");
    },
  },
};
</script>

<style scoped>
/* TodoFooter的样式 */
.to-do-list-footer {
  height: 32px;
  line-height: 32px;
  padding-left: 8px;
  margin-top: 16px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.to-do-list-footer label {
  cursor: pointer;
}

.to-do-list-footer label input {
  vertical-align: middle;
  position: relative;
  margin-right: 8px;
  top: -1px;
}
</style>
