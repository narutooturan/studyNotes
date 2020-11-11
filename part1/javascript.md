# javascript

- observer
```
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
- 事件
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
  - mouseenter 和 mouseleave：只要进入或离开 dom 就会被调用
  - mouseover 和 mouseout：只要进入或离开 dom 及其子元素均会被调用
- async 与 await 需要成对出现
- 检查文件格式
```javascript
if (file.type.indexOf('/png') == -1) {
  // ...
}
```
