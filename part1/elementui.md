# elementUI

- 日期下拉框闪屏问题
由于 .popper__arrow 引起，因此在 dom 加载完成后，删除 .popper__arrow dom
```javascript
this.$nextTick(() => {
  document.querySelector('.popper__arrow').remove()
})
```