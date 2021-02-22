# Vue

## 性能

- interval 需要在 beforeDestroy 中 clear
- eventbus 需要在 beforeDestroy 中 off
  - 如果不在 beforeCreate 时 $eventBus.$off 会导致多次触发

## 数组

- 更新数组时，Vue可能存在不响应的情况，可以通过以下方式进行更新。
  - ` array.push() `
  - ` Vue.set(array, index, value) `

- 数组对象属性的单独修改也不会响应。

## 监听事件

- 监听组件内生命周期事件
```javascript
this.$once('hook:created', () => {})
```

- 监听其他组件生命周期事件
```javascript
<component @hook:created=""/>
```

## transition

使用 transition，需要定义 name 属性，需要为该属性设置效果。
```vue
<transition name="fade" >
  <div v-if="ifShow">text</div>
</transition>

<style>
  .fade-enter-active, .fade-leave-active {
    transition: opacity .3s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
</style>
```

## 自定义组件

### 动态添加组件

```javascript
// 配置 vue.config.js 允许使用 extend template 新建 dom
module.export = {
  runtimeCompiler: true,
}

// 新建div
let div = Vue.extend({
  props:{},
  data () {return {}},
  template: `<div @click="">{{data}}</div>`,
  methods: {
    method () { this.$emit() }
  }
})

// 动态添加 div
let newDom = new div({
  propsData: {}
})
newDom.$on()
dom.appendChild(newDom)
```

### 实现 v-model 双向绑定

- 监听上级组件，如果发生变化，更新 v-model 值。
- 使用 watch 监听自定义组件 v-model 值，如果发生变化触发上级组件 input 和 on-change 事件。
```vue
<sub>
    <input v-model="model" />
</sub>
props: { value },
watch: {
  value () {
    model = value
  },
  model (val) {
    $emit('input', val)
    $emit('on-change', val)
  }
}

<parent v-model="value" />
```

### 实现自定义事件

- 自定义组件中监听 click 等事件。
- 如果用户触发 click 等事件，则使用 $emit 触发上级组件的自定义事件。
```vue
<sub @click="clickEvent" />
clickEvent () {
  $emit('clickEvent')
}

<parent @clickEvent="handleEvent" />
```

## keep alive

- 使用 keep-alive 只会调用一次 created 方法
- 使用 keep-alive 切换组件时，会调用 activated 方法和 deactivated 方法
  - 如果 activated 方法和 deactivated 方法中包含请求，请求参数存在变化才会请求接口

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

### 父子组件之间传值

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

## 其他

- 在组件中使用 ``` :key ``` 进行标识，可以触发组件重新渲染
- mock 数据
  - 安装 mockjs
  - 新建 mock 文件夹，在 mock.js 文件中 import 其他模块的 mock 文件
  - 在 main.js 中引入 mock.js
  - 在 axios 中配置本地地址（请求本地即可获取mock数据，mock数据不会产生请求，无法从network中查看）
- 动态调用 img
```vue
<img :src="image">

try {
  this.image = require(`@/assets/images/${this.url}`)
} catch(){
  this.image = require(`@/assets/images/default.png`)
}
```