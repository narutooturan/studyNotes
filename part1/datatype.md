# 数据类型

## 数据类型

- Number
- Boolean
- String
- Null
- Undefined
- Object
- Symbol

## 数据类型检测

```javascript

typeof 42   === 'number'
typeof true === 'boolean'
typeof '42' === 'string'
typeof undefined        === 'undefined'
typeof { key: 'value' } === 'object'
typeof Symbol() === 'symbol'
a === null

// 特殊情况
typeof null === 'object'
// 复合方式检查 null
(!null && typeof null === 'object')
// true

```
