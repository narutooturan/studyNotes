# javascript

# string

- 替换字符串所有指定关键词
```javascript
str.replace(/a/g, 'b');
```

## 动画效果

- 使用 class 添加动画效果，如果需要停止后重新启动动画，需要在删除 class 后停顿一会儿
```javascript
// 添加动画效果
dom.classList.add('animation')
// 停止动画效果
dom.classList.remove('animation')
// 重新启动动画效果
setTimeout(() => {
  dom.classList.add('animation')
}, 500)
```

## 事件

- pjax重复加载js 导致绑定点击事件无限重复
```javascript
$(".demo").off().on("click", function () {
});
```

- 判断是否向下滚动
```javascript
let delta = (e.event.wheelDelta && (e.event.wheelDelta>0?1:-1)) || (e.event.detail && (e.event.detail>0?-1:1))
if (delta < 0) {
  // 向下滚动
} else {
  // 向上滚动
}
```

- target & currentTarget
  - target 是当前触发元素
  - currentTarget 是实际绑定元素

- 自定义事件
```javascript
// define event
var event = new customEvent('eventName', {
  detail: {
    param: '',
  }
});
// dispatch event
document.querySelector().dispatchEvent(event);
```

- mouse 事件
  - mouseenter 和 mouseleave：只要进入或离开 dom 就会被调用
  - mouseover 和 mouseout：只要进入或离开 dom 及其子元素均会被调用

## 其他

- async 与 await 需要成对出现

- 检查文件格式
```javascript
if (file.type.indexOf('/png') == -1) {
  // ...
}
```

- classList
  - add()
  - remove()

- document.excuteCommand fontSize 支持参数为 1-7

- observer
```javascript
var myElement = $("<div>hello world</div>")[0];

var observer = new MutationObserver(function(mutations) {
   if (document.contains(myElement)) {
        console.log("It's in the DOM!");
        observer.disconnect();
    }
});

observer.observe(document, {attributes: false, childList: true, characterData: false, subtree:true});

$("body").append(myElement); // console.log: It's in the DOM!
```

- 如果重新赋值dom，会影响之前的js事件响应
- 打开新链接
  - 新建 a dom 对象，触发其点击事件