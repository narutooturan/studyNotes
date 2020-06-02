# Vue

## 事件

可能存在无法响应原生事件的情况，使用修饰符 ` @click.native="" ` 解决。

## 传值

### 组件传值

在 Vue 原型链中加入 eventhub，统一从 eventhub 中触发事件，响应事件。

```javascript

// main.js
Vue.prototype.$eventHub = new Vue()
// page
this.$eventHub.$emit('refresh')
this.$eventHub.$on('refresh', () => {})

```

### 兄弟组件之间传值

- 子组件 A 使用 emit 触发事件

```javascript

this.$emit('event', params)

```

- 子组件 A 处理触发事件，修改子组件 B props 属性

```javascript

<compA @event="handleEvent" />
handleEvent (params) {
  this.b = params
}

<compB :b="b" />

```

- 子组件 B 使用 watch 监听 props 属性，执行操作

```javascript

watch: {
  b () {
    // handle
  }
}

```