# react

- 更新对象（嵌套）属性

```javascript

this.setState({
  ...this.state,
  person: {
    ...this.state.person,
    age: 0
  }
})

```