# Reflect

## 基本介绍

- Reflect 是一个内置对象。
- 它提供方法（同 proxy handler 方法）来拦截 JS 操作。
- 它不是构造函数，因此是不可构造的（无法使用 new 或将其作为函数调用）。
- 它的所有属性和方法都是静态的。

## 静态方法

### ` Reflect.construct(target, argumentList[, newTarget]) `

- 描述

允许用户使用参数来调用构造函数，相当于调用 ` new target(...args) `。

``` javascript

let obj = new Foo(...args)
let obj = Reflect.construct(Foo, args)

```

- 参数
  - target: 需要调用的目标函数。
  - argumentsList: 类数组对象；target 调用的参数列表。
  - newTarget: 可选；新创建对象的原型对象。

- 返回值
  - 以 target 为构造函数，argumentsList 为参数的实例对象。

- 异常
  - 如果 target 或 newTarget 不是构造函数，那么抛出 TypeError。

- 基本使用

```javascript

function func1(a, b, c) {
  this.sum = a + b + c;
}

const args = [1, 2, 3];
const object1 = new func1(...args);
const object2 = Reflect.construct(func1, args);

console.log(object1.sum); // output: 6
console.log(object2.sum); // output: 6

```

- `Reflect.construct` vs ` Object.create() `

在 Reflect 对象出现之前，通过组合原型对象和构造函数的形式调用 ` Object.create() ` 实现对象创建。

```javascript

function OneClass() { this.name = 'one' }
function OtherClass() { this.name = 'other' }

// two ways to create object
// use Reflect
let obj1 = Reflect.construct(OneClass, args, OtherClass)
// use Object.create()
// the same as use Reflect
let obj2 = Object.create(OtherClass.prototype)
OneClass.apply(obj2, args)

console.log(obj1.name)
// 'one'
console.log(obj2.name)
// 'one'

console.log(obj1 instanceof OneClass)
// false
console.log(obj2 instanceof OneClass)
// false

console.log(obj1 instanceof OtherClass)
// true
console.log(obj2 instanceof OtherClass)
// true

```

两种方式几乎相同，但在创建过程中仍然存在细微差别。如果使用 ` Object.create() ` 和 ` Function.prototype.apply() ` （没有使用 new 操作符），那么构造函数内部的 new.target 值会指向 undefined。如果使用 ` Reflect.construct() `，那么构造函数内部的 new.target 会指向 newTarget 对象（如果指定了 newTarget 对象）或 target 对象。

```javascript

function OneClass() {
  console.log('OneClass')
  console.log(new.target)
} 
function OtherClass() {
  console.log('OtherClass')
  console.log(new.target)
}

let obj1 = Reflect.construct(OneClass, args)
// Output:
// OneClass
// function OneClass { ... }

let obj2 = Reflect.construct(OneClass, args, OtherClass)
// Output:
// OneClass
// function OtherClass { ... }

let obj3 = Object.create(OtherClass.prototype);
OneClass.apply(obj3, args)
// Output:
// OneClass
// undefined

```

### ` Reflect.get(target, propertyKey[, receiver]) `

- 描述

允许用户通过调用函数的形式，读取对象的属性。其效果类似于 ` target[propertyKey] `。

```javascript

// object
const object1 = { x: 1, y: 2 };
console.log(Reflect.get(object1, 'x'));
// expected output: 1

// array
const array1 = ['zero', 'one'];
console.log(Reflect.get(array1, 1));
// expected output: "one"

```

- 参数
  - target: 需要获取属性的目标对象。
  - propertyKey: 需要获取的属性名。
  - receiver: 可选；this 值（如果 target 指定了 getter）；同 proxy 共同使用时，它为一个继承自 target 的对象。

- 返回值
  - 属性值

- 异常
  - 如果 target 不是对象，则抛出错误 TypeError。

- 基本使用

```javascript

// Object 
let obj = { x: 1, y: 2 }
Reflect.get(obj, 'x')
// 1

// Array
Reflect.get(['zero', 'one'], 1)
// "one"

// Proxy with a get handler
let x = {p: 1};
let obj = new Proxy(x, {
  get(t, k, r) { return k + 'bar' }
})
Reflect.get(obj, 'foo')
// "foobar"

//Proxy with get handler and receiver
let x = {p: 1, foo: 2};
let y = {foo: 3};
let obj = new Proxy(x, {
  get(t, prop, receiver) {
    return receiver[prop] + 'bar'
  }
})
Reflect.get(obj, 'foo', y)
// "3bar"

```

### ` Reflect.set(target, propertyKey, value[, receiver]) `

- 描述

允许用户通过调用函数的方式设置对象的属性。

```javascript

// object
const object1 = {};
Reflect.set(object1, 'property1', 42);
console.log(object1.property1);
// expected output: 42

// array
const array1 = ['duck', 'duck', 'duck'];
Reflect.set(array1, 2, 'goose');
console.log(array1[2]);
// expected output: "goose"

```

- 参数
  - target: 需要设置属性的目标对象。
  - propertyKey: 属性名。
  - value: 属性值。
  - receiver: 可选；this 值（如果 target 指定了 setter）。

- 返回值
  - 返回布尔值。
  - 表明设置属性是否成功。

- 异常
  - 如果 target 不是一个对象，则抛出错误 TypeError。

- 基本使用

```javascript

// Object
let obj = {}
Reflect.set(obj, 'prop', 'value')
// true
obj.prop
// "value"

// Array
let arr = ['duck', 'duck', 'duck']
Reflect.set(arr, 2, 'goose')
// true
arr[2]
// "goose"

// It can truncate an array. 
// 截断一个数组
Reflect.set(arr, 'length', 1)
// true
arr
// ["duck"]

// With just one argument, propertyKey and value are "undefined".
let obj = {}
Reflect.set(obj)
// true
Reflect.getOwnPropertyDescriptor(obj, 'undefined')
// { value: undefined, writable: true, enumerable: true, configurable: true }

```

### ` Reflect.apply(target, thisArgument, argumentList) `

- 描述

允许用户通过指定参数调用目标函数。该方法类似于使用 ` Function.prototype.apply() ` 来调用函数（显式地指定 this 和参数列表）。使用 ` Reflect.apply() ` 会使代码更加简洁易懂。

```javascript

Function.prototype.apply.call(Math.floor, undefined, [1.75]);
// expected output: 1
Reflect.apply(Math.floor, undefined, [1.75]);
// expected output: 1

Reflect.apply(String.fromCharCode, undefined, [104, 101, 108, 108, 111]);
// expected output: "hello"

Reflect.apply(RegExp.prototype.exec, /ab/, ['confabulation']).index;
// expected output: 4

Reflect.apply(''.charAt, 'ponies', [3]);
// expected output: "i"

```

- 参数
  - target: 需要调用的目标函数。
  - thisArgument: 目标函数调用时绑定的 this 值。
  - argumentsList: 类数组对象；目标函数调用的参数列表。

- 返回值
  - 返回根据 this 和参数列表调用的目标函数的运行结果。

- 异常
  - 如果 target 函数无法被调用，则抛出错误 TypeError。

- 基本使用

```javascript

Reflect.apply(Math.floor, undefined, [1.75]);
// 1;

Reflect.apply(String.fromCharCode, undefined, [104, 101, 108, 108, 111])
// "hello"

Reflect.apply(RegExp.prototype.exec, /ab/, ['confabulation']).index
// 4

Reflect.apply(''.charAt, 'ponies', [3])
// "i"

```

### ` Reflect.has(target, propertyKey) `

- 描述

允许用户通过调用函数的方式实现类似于 in 操作符的效果。

```javascript

const object1 = { property1: 42 };

console.log(Reflect.has(object1, 'property1'));
// expected output: true

console.log(Reflect.has(object1, 'property2'));
// expected output: false

console.log(Reflect.has(object1, 'toString'));
// expected output: true

```

- 参数
  - target: 需要寻找属性的目标对象。
  - propertyKey: 属性名。

- 返回值
  - 返回布尔值。
  - 表示属性是否存在于目标对象中。

- 异常
  - 如果 target 不是一个对象，则抛出错误 TypeError。

- 基本使用

```javascript

Reflect.has({x: 0}, 'x')
// true
Reflect.has({x: 0}, 'y')
// false

// returns true for properties in the prototype chain
Reflect.has({x: 0}, 'toString')

// Proxy with .has() handler method 
obj = new Proxy({}, {
  has(t, k) {
    return k.startsWith('door')
  }
);

Reflect.has(obj, 'doorbell')
// true
Reflect.has(obj, 'dormitory')
// false

```

- 嵌套

` Reflect.has() ` 针对任何层级的嵌套属性均会返回值，同 in 操作符。

```javascript

const a = {foo: 123}
const b = {__proto__: a}
const c = {__proto__: b}

// The prototype chain is: c -> b -> a
Reflect.has(c, 'foo')
// true

```