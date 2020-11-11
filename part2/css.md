# CSS

- point-events 设为 none，可以穿透当前 dom，监听下一层 dom 的事件
- height
  - 如果存在小数 height 或 float 未 clear，可能发生部分内容消失，更改 height 为单数或偶数即可显示
- 可使用 drop-shadow 修改 icon 颜色
  - 如果使用 `overflow: hidden` 或 `opacity: 0`，等方式将原有 icon 遮挡或隐藏，则其阴影也不在显示；可使用新的 dom 配合 `z-index` 属性覆盖原有 icon 实现；
- rem
  - 相对于 html font-size 来定
  - 可以在 body 中规定实际使用的 font-size，在 html 中规定 rem 使用的 font-size
- border-radius 失效
  - width，height 为单数时会失效
- span:before content 图片垂直居中
```
display: inline-block;
vertical-align: middle;
line-height: normal;
```
- 如何让Chrome浏览器支持小于12px的字体大小？
` -webkit-transform: scale(0.8); `
- flex
  - flex-grow 默认为 0；0，不扩展宽度；
  - flex-shrink 默认为1；0，不压缩宽度；
  - 如果使用 `flex-grow` 和 `flex-shrink` 自动计算各部分宽度，可能会存在小数点宽度，可能会因此遮挡部分内容；可以设置某些部分固定宽度；
- 单行文本省略号
```
overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;
```