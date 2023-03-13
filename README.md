# 基于vue2.0+localStorage开发的TodoList

项目预览：https://theresa45-to-do-list.netlify.app/

## 功能说明

-支持Enter添加任务
-支持任务状态切换：已完成(勾选checkbox)，未完成(取消勾选checkbox)
-支持编辑任务
-支持删除任务
-支持任务数据汇总
-支持清空所有已完成的任务
-支持本地化存储

## 项目安装步骤

```
git clone https://github.com/Theresa45/vue-to-do-list.git
npm install

// 开发阶段的编译和热加载
npm run serve

// 构建最终的生产版本
npm run build
```
## 主要难点：组件间的通信

父组件传递数据给子组件可以通过`props`配置项实现。

子组件传递数据给父组件可以通过绑定自定义事件实现。

祖孙关系的组件之间的通信则可以通过全局事件总线实现
