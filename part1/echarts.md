# echarts

- 自适应窗口大小
  - 需要在 setOption 后隔段时间重新使用 resize 调整大小
  - 在 window.resize 时间中添加 resize 方法
```javascript
chart.setOption(option)
setTimeout(() => {
  chart.resize()
}, 500)
```

- echarts reload
  - 需要使用 clear 或 destroy 销毁后，重新使用 setOption 初始化
```javascript
chart.clear()
chart.setOption(option)
```

- 伪3d bar效果
  - 使用 extendShape 拼接3面，声明完后需要使用 registerShape 注册自定义类型
  - [参考](https://www.makeapie.com/editor.html?c=xgbq3iH40k)