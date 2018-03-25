redux
====


# action

Action 是把数据从应用传到 store 的有效载荷。它是 store 数据的唯一来源。通过 store.dispatch() 将 action 传到 store。

`{ type: TOGGLE_TODO, index: 5 }`

# Action 创建函数

``` js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}

dispatch(addTodo(text))

```

# reducers

使用纯函数来执行修改
为了描述 action 如何改变 state tree ，你需要编写 reducers

`reducers`: Reducer 只是一些纯函数，它接收先前的 state 和 action，并返回新的 state。

为了描述 action 如何改变 state tree ，你需要编写 reducers。

``` js
function visibilityFilter(state = 'SHOW_ALL', action) {
 switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
}

function todos (state = [], action) {

}
import { combineReducers, createStore } from 'redux'
let reducer = combineReducers({ visibilityFilter, todos })
let store = createStore(reducer)
```


# 异步 Action

当调用异步 API 时，每个 API 请求都需要 dispatch 至少三种 action

- 异步 action 创建函数

``` js
export function fetchPosts(subreddit) {
  return function (dispatch) {
    // 首次 dispatch：更新应用的 state 来通知
    // API 请求发起了。
    dispatch(requestPosts(subreddit))
    return fetch(`http://www.subreddit.com/r/${subreddit}.json`)
      .then(
        response => response.json(),
        error => console.log('An error occurred.', error)
      )
      .then(json =>
        // 可以多次 dispatch！
        // 这里，使用 API 请求结果来更新应用的 state。
        dispatch(receivePosts(subreddit, json))
      )
  }
}
```

# Store

- 维持应用的 state；

- 提供 getState() 方法获取 state；

- 提供 dispatch(action) 方法更新 state；

- 通过 subscribe(listener) 注册监听器;

- 通过 subscribe(listener) 返回的函数注销监听器。

# react-redux 

容器组件 和 展示组件

<Provider store>
connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
connectAdvanced(selectorFactory, [connectOptions])

``` js
const mapStateToProps = (state, ownProps) => {
  return { todos: state.todos }
}
function mapDispatchToProps(dispatch) {
  return {
    todoActions: bindActionCreators(todoActionCreators, dispatch),
    counterActions: bindActionCreators(counterActionCreators, dispatch)
  }
}
export default connect(mapStateToProps, mapDispatchToProps)(TodoApp)
```