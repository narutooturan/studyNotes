# Proxy

## 基本使用

使用 Proxy 可以对 target（目标对象）拦截某些操作并实现自定义行为 handler。

```javascript

// 如果对象中存在属性，那么返回该属性；如果不存在，返回42。
let handler = {
  get: function (target, name) {
    return name in target ? target[name] : 42;
  }
};

// Proxy 对象定义了一个目标（这里是一个空对象）和一个实现了 get 陷阱的 handler 对象。
let p = new Proxy({}, handler);
p.a = 1;

// target 为 p，name 为 a。
// 1, 42
console.log(p.a, p.b);

```

## 其他使用方式

- 设置 object.proxy 属性，从而在 object 上调用。

```javascript

var target = {};
var handler = {
  get (target, name) {
    return '1';
  }
};

var proxy = {
  proxy: new Proxy(target, handler)
};

// proxy.proxy.a 为 1

```

- Proxy 实例作为其他对象的原型对象，其他对象通过原型链调用 proxy。

```javascript

var proxy = new Proxy(target, handler);
let obj = Object.create(proxy);

// obj.a 为 1

```

## handler 支持的拦截方法

- [mdn proxy handler list](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler)

### construct

用于拦截 new 方法。

#### 参数

- target：目标对象（如 {} 空对象）
- argumentsList：constructor的参数列表。
- newTarget：最初被调用的构造函数（如应用 1 为 p ）。

#### 返回值

construct 方法必须返回一个对象。

#### 拦截

- new proxy(...args)
- Reflect.construct()

#### 约束

- 如果未返回一个对象，则抛出错误 TypeError。
- 为了使 new 操作符在生成的 Proxy 对象上生效，用于初始化代理的目标对象自身必须具有 Construct 内部方法（即 new target 必须是有效的）。

#### 应用

- 基础使用

```javascript

// newTarget 为 p
var p = new Proxy(function () {}, {
  construct: function(target, args) {
    console.log('called: ' + args.join(', '));
    return { value: args[0] * 10 };
  }
});

(new p(1)).value
// "called: 1"
// 10

```

### get(target, propertyName, receiver)

用于拦截对象的读取属性操作。

#### 参数

this 上下文绑定在 handler 对象上。

- target：目标对象（如 {} 空对象）
- propertyName：被获取的属性名。
- receiver：Proxy 或者继承 Proxy 的对象；可选

#### 返回值

get 方法可以返回任何值。

#### 拦截

- 访问属性: proxy[foo] 和 proxy.bar
- 访问原型链上的属性: Object.create(proxy)[foo]
- Reflect.get()

#### 约束

- 如果要访问的目标属性是不可写（writable）以及不可配置（configurable）的，则返回的值必须与该目标属性的值相同，否则抛出错误 TypeError。。
- 如果要访问的目标属性没有配置访问方法，即 get 方法是 undefined 的，则返回值必须为 undefined，否则抛出错误 TypeError。。

#### 应用

- 实现数组读取负数的索引

```javascript

function createArray(...elements) {
  let handler = {
    get(target, propKey, receiver) {
      let index = Number(propKey);
      if (index < 0) {
        propKey = String(target.length + index);
      }
      return Reflect.get(target, propKey, receiver);
    }
  };

  let target = [];
  target.push(...elements);
  return new Proxy(target, handler);
}

let arr = createArray('a', 'b', 'c');
arr[-1] // c

```

- 实现属性链式操作

```javascript

var pipe = function (value) {
  var funcStack = [];
  var oproxy = new Proxy({} , {
    get : function (pipeObject, fnName) {
      if (fnName === 'get') {
        return funcStack.reduce(function (val, fn) {
          return fn(val);
        },value);
      }
      funcStack.push(window[fnName]);
      return oproxy;
    }
  });

  return oproxy;
}

var double = n => n * 2;
var pow    = n => n * n;
var reverseInt = n => n.toString().split("").reverse().join("") | 0;

pipe(3).double.pow.reverseInt.get; // 63

```

- 实现一个生成各种 DOM 节点的通用函数dom

```javascript

const dom = new Proxy({}, {
  get(target, property) {
    return function(attrs = {}, ...children) {
      const el = document.createElement(property);
      for (let prop of Object.keys(attrs)) {
        el.setAttribute(prop, attrs[prop]);
      }
      for (let child of children) {
        if (typeof child === 'string') {
          child = document.createTextNode(child);
        }
        el.appendChild(child);
      }
      return el;
    }
  }
});

const el = dom.div({},
  'Hello, my name is ',
  dom.a({href: '//example.com'}, 'Mark'),
  '. I like:',
  dom.ul({},
    dom.li({}, 'The web'),
    dom.li({}, 'Food'),
    dom.li({}, '…actually that\'s it')
  )
);

document.body.appendChild(el);

```

### set

用于拦截某个属性的赋值操作。

#### 参数

- target：目标对象（如 {} 空对象）
- propertyName：将被设置的属性名或 Symbol
- propertyValue：新属性值
- receiver：最初被调用的对象（通常是 proxy 本身），但 handler 的 set 方法也有可能在原型链上，或以其他方式被间接地调用（因此不一定是 proxy 本身）；可选

> 比如：假设有一段代码执行 obj.name = "jen"， obj 不是一个 proxy，且自身不含 name 属性，但是它的原型链上有一个 proxy，那么，那个 proxy 的 set() 处理器会被调用，而此时，obj 会作为 receiver 参数传进来。

```javascript

var proxy = new Proxy({}, {
  get (target, property) {
    console.log('get')
    return 'get'
  },
  set (target, property, value) {
    console.log('set')
    return 'set'
  }
})

var obj = Object.create(proxy)

// 此时 obj 的 receiver === obj，而不是 proxy 本身，因为此时调用原型链的 set 方法
obj.a = 1
// 此时 proxy 的 receiver === proxy
proxy.a = 1

```

#### 返回值

- set() 方法应当返回一个布尔值。
- 返回 true 代表属性设置成功。
- 在严格模式下，如果 set() 方法返回 false，那么会抛出一个 TypeError 异常。

#### 拦截

- 指定属性值：proxy[foo] = bar 和 proxy.foo = bar
- 指定继承者的属性值：Object.create(proxy)[foo] = bar
- Reflect.set()

#### 约束

- 若目标属性是一个不可写（writable）及不可配置（configurable）的数据属性，则不能改变它的值，否则抛出 TypeError 异常。
- 如果目标属性没有配置存储方法，即 [[Set]] 属性的是 undefined，则不能设置它的值，否则抛出 TypeError 异常。
- 在严格模式下，如果 set() 方法返回 false，那么也会抛出一个 TypeError 异常。

#### 应用

- 变量验证

```javascript

let validator = {
  set: function(obj, prop, value) {
    if (prop === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError('The age is not an integer');
      }
      if (value > 200) {
        throw new RangeError('The age seems invalid');
      }
    }

    // 对于满足条件的 age 属性以及其他属性，直接保存
    obj[prop] = value;
  }
};

let person = new Proxy({}, validator);

person.age = 100;

person.age // 100
person.age = 'young' // 报错
person.age = 300 // 报错

```

- 利用 set 方法进行数据绑定，即当对象发生变化时，自动更新 dom。

- 结合 get 和 set 方法，防止内部属性被外部读写。

```javascript

const handler = {
  get (target, key) {
    invariant(key, 'get');
    return target[key];
  },
  set (target, key, value) {
    invariant(key, 'set');
    target[key] = value;
    return true;
  }
};
function invariant (key, action) {
  if (key[0] === '_') {
    throw new Error(`Invalid attempt to ${action} private "${key}" property`);
  }
}
const target = {};
const proxy = new Proxy(target, handler);
proxy._prop
// Error: Invalid attempt to get private "_prop" property
proxy._prop = 'c'
// Error: Invalid attempt to set private "_prop" property

```

### apply

用于拦截函数调用，apply 和 call 方法。

#### 参数

- target：目标对象（函数）
- ctx：被调用时的上下文对象（ this ）
- args：被调用时的参数数组

#### 返回值

apply 方法可以返回任何值。

#### 拦截

- proxy(...args)
- Function.prototype.apply() 和 Function.prototype.call()
- Reflect.apply()

#### 约束

- target 必须是可被调用的。也就是说，它必须是一个函数对象，否则抛出 TypeError 异常。

#### 应用

- 统一修改函数实现

```javascript

var twice = {
  apply (target, ctx, args) {
    return Reflect.apply(...arguments) * 2;
  }
};
function sum (left, right) {
  return left + right;
};
var proxy = new Proxy(sum, twice);
proxy(1, 2) // 6
proxy.call(null, 5, 6) // 22
proxy.apply(null, [7, 8]) // 30

```

### has

用于拦截 hasProperty 操作。即判断对象是否存在某属性的操作，如 in 运算符。

#### 参数

- target：目标对象（如 {} 空对象）
- propertyName：需要检查是否存在的属性

#### 返回值

has 方法返回一个 boolean 属性的值。

#### 拦截

- 属性查询: foo in proxy
- 继承属性查询: foo in Object.create(proxy)
- with 检查: with(proxy) { (foo); }
- Reflect.has()

#### 约束

- 如果目标对象的某一属性本身不可被配置（ configurable ），则该属性不能够被代理隐藏，否则抛出 TypeError 异常。
- 如果目标对象为不可扩展对象（ ``` Object.preventExtensions(obj) ``` ），则该对象的属性不能够被代理隐藏，否则抛出 TypeError 异常。
- has 方法拦截的是 hasProperty 方法，而不是 hasOwnProperty 方法，因此无法区分属性是否为继承属性。
- has 拦截对于 for...in 循环不生效。

#### 应用

- 使用has方法隐藏某些属性，不被in运算符发现。

```javascript

var handler = {
  has (target, key) {
    if (key[0] === '_') {
      return false;
    }
    return key in target;
  }
};
var target = { _prop: 'foo', prop: 'foo' };
var proxy = new Proxy(target, handler);
'_prop' in proxy // false

```

## 撤销 revoke

Proxy.revocable() 方法被用来创建可撤销的 Proxy 对象。这意味着 proxy 可以通过 revoke 函数来撤销，并且关闭代理。此后，代理上的任意的操作都会导致TypeError。

### 应用

- 目标对象不允许直接访问，必须通过代理访问，一旦访问结束，就收回代理权，不允许再次访问。

```javascript

var revocable = Proxy.revocable({}, {
  get: function(target, name) {
    return "[[" + name + "]]";
  }
});
var proxy = revocable.proxy;
console.log(proxy.foo); // "[[foo]]"

revocable.revoke();

console.log(proxy.foo); // TypeError is thrown
proxy.foo = 1           // TypeError again
delete proxy.foo;       // still TypeError
typeof proxy            // "object", typeof doesn't trigger any trap

```
