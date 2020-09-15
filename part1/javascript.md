# javascript

- 如果重新赋值dom，会影响之前的js事件响应
- 事件
  - mouseenter 和 mouseleave：只要进入或离开 dom 就会被调用
  - mouseover 和 mouseout：只要进入或离开 dom 及其子元素均会被调用
- async 与 await 需要成对出现
- 检查文件格式

```javascript

if (file.type.indexOf('/png') == -1) {
  // ...
}

```
