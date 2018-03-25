


# mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation

store.commit('increment')

载荷形式 & 对象形式

``` js
this.$store.commit('incrementAsync', {
  amount: 10
})
```

# Action

Action 提交的是 mutation，而不是直接变更状态。
Action 可以包含任意异步操作。

``` js
actions: {
  increment (context) {
    context.commit('increment')
  }
}

this.$store.dispatch('incrementAsync', {amount: 10})
store.dispatch({type: 'incrementAsync',amount: 10})
```

# 表单处理

双向绑定的计算属性

``` js
computed: {
  message: {
    get () {
      return this.$store.state.obj.message
    },
    set (value) {
      this.$store.commit('updateMessage', value)
    }
  }
}
```