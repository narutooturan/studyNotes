# Meta Programming 元编程

元编程在语言层面做出修改，即对编程语言进行编程。

ES6中，新增了 Proxy 和 Reflect 对象，允许拦截并定义基本语言操作的自定义行为（如属性查找，赋值，枚举，函数调用等）。借助这两个对象，可以实现元编程。