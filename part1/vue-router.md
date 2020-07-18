# vue-router

## 开发记录

- ` keep-alive `
  - 问题：在组件间切换时，有时会想保持这些组件的状态，以避免反复重渲染导致的性能问题；如，保存输入框中的值
  - 原因：每次切换组件时，vue 都会创建一个新的实例，因此无法存储数据
  - 解决：使用 ` <keep-alive> ` 元素将动态组件包裹起来可以解决该问题

- 监听 router 变化

可能存在无法监听的情况，可使用多种方式。原因未知。

```javascript

// method 1
watch: {
  $route: (to, from) {
  }
}

// method 2
watch: {
  $route: {
    handler (to, from) {
    },
    deep: true
  }
}

// method 3
beforeRouteEnter (to, from, next) {
  next(vm => {
  })
}

```