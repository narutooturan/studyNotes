# vuex

- 何时使用vuex：当多个组件依赖同一个状态时，使用 vuex
- vuex 使用 logger 打印 vuex 操作记录
- vuex-persist
  - 问题：存储在 Vuex store 中的数据，只要刷新页面，数据就会丢失
  - 原因：Vuex 只解决了多视图之间的数据共享问题，无法保存页面数据至刷新后
  - 解决：vuex-persist 不需要你手动存取 storage ，而是直接将状态保存至 cookie 或者 localStorage 中