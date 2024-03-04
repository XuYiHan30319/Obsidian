# redux

Redux独立于组件树,不需要组件直接交换通信,所以Redux可以管理公有状态,比如用户信息

- 定义一个reducer函数,根据当期需要返回更改后的状态
- 使用createStore函数传入reducer函数,生成store实例对象
- 使用store的subscribe方法订阅变化,一旦发生变化就立即通知
- 通过dispatch方法提交action修改数据
- getState获得数据状态

state:存放数据对象

action:修改对象

reducer:根据action修改state



```shell
bun i @reduxjs/toolkit react-redux
```

toolkit用来简化编写方式

React-redux中间件,用于链接redux和react

->counterStore.js

```react
import { createSlice } from "@reduxjs/toolkit";

const counterStore = createSlice({
    name: "counter",
    initialState: {
        count: 0,
    },
    //修改数据的方法,支持直接修改
    reducers: {
        increase(state) {
            state.count++;
        },
        decrease(state) {
            state.count--;
        },
	      addToNum(state, action) {
            state.count = action.payload
        }
    }
})

const { increase, decrease } = counterStore.actions;

const reducer = counterStore.reducer;

export { increase, decrease };
export default reducer
```

->index.js

```react
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from './modules/counterStore';

const store = configureStore({
    reducer: {
        counter: counterReducer
    }
})

export default store;
```

->app.js

```react
import { useDispatch, useSelector } from "react-redux";
import { decrease, increase } from "./store/modules/counterStore";

function App() {

  const { count } = useSelector(state => state.counter);
  const dispath = useDispatch();

  return (
    <div className="App">
      <button onClick={() => { dispath(addToNum(20) }}>20</button>
      <button onClick={() => { dispath(increase()) }}>+</button>
      {count}
      <button onClick={() => { dispath(decrease()) }}>-</button>
    </div >
  );
}

export default App;

```

->index.js

```react
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { Provider } from 'react-redux';
import store from './store';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);

```

