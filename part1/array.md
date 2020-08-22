# Array

- nodelist 转 array
```
[].slice.call(nodelist)
```
- 新建副本
```javascript
let array1 = array2.slice()
```
- 拷贝
```
// 一维，浅拷贝
array.slice()
// 一维，浅拷贝
Object.assign({}, obj)
// 可实现多维，但会忽略 undefined，函数，symbol 值
JSON.parse(JSON.stringify(obj))
// 深拷贝
通过递归方式
```
- map
  - 需要传入 this
  - 返回新的数组，不修改原数组
- filter
  - 不返回新的数组，不修改原数组