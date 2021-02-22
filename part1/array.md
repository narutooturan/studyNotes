# Array

- 根据长度生成数组
```javascript
const arr0 = [...new Array(3).keys()];
const arr1 = new Array(5).fill({name:'linda', age: 18, sex: 1})
```

- 获取最大值，最小值
```javascript
let a = [0,1,2]
Math.max(...a)
```

- 遍历 key / value
```javascript
let a = {a:0,b:1}
Object.keys(a).forEach()
Object.values(a).forEach()
```

- concat 不会改变原数组，需要重新赋值

- 获取数组中对象某个key对应的所有值
```javascript
var user = [
 	{
         id: 1,
         name: "李四"
     },
     {
         id: 2,
         name: "张三"
     },
     {
         id: 3,
         name: "李五"
     }
 ]
var userName = Array.from(user,({name})=>name);
console.log(userName); // ["李四", "张三", "李五"]
```

- nodelist 转 array
```javascript
[].slice.call(nodelist)
```

- 拷贝
```javascript
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

- 数组转字符串
```javascript
array.toString()
array.join(',')
```