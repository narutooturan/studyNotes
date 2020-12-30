# 正则表达式

## 新建正则表达式

### 基础方式
```javascript
// 方法 1
let pattern = /1/ig
// 方法 2
let pattern = new Reg('1', 'ig')
```

### 动态方式

- 使用 for-in，结合 ` new RegExp() ` 循环生成正则表达匹配
```javascript
let flag = false
for ( let index in paths ) {
  let path = paths[index]
  let pattern = new RegExp('/' + path + '$', 'ig')
  flag = flag || pattern.test(url);
}
```

## 修饰符
```javascript
// i: ignoreCase 区别大小写
// g: 全局搜索
```
