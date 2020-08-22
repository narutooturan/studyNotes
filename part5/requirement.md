# 业务需求

## 发送验证码

```javascript

...
generateCodeText () {
  const TIME = 60
  var timeCount = TIME
  timer = setInterval(() => {
    if (timeCount>0 && timeCount<=TIME) {
      timeCount--
      codeText = timeCount + 's'
    } else {
      clearInterval(timer)
      ifSend = false
      codeText = '发送验证码'
    }
  }, 1000)
},
handleCodeSend () {
  if (ifSend) return

  send().then((res) => {
    if (res.code == 200) {
      ifSend = true
      generateCodeText()
    }
  })
}
...

```