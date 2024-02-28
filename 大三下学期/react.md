# React

index.js是启动口,代码通过bable解析

```react
//一切组件的根基
function App() {
  return (
    <div className="App">
      this is app
    </div>
  );
}

export default App;


//必要的核心包
import React from 'react';
import ReactDOM from 'react-dom/client';

//导入根组件
import App from './App';

//把App渲染到id为root的 DOM节点上 Public/index.html/root
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <App />
);
```

Jsx:在js中写html,同时享用js的可编程能力与html声明式模版

react使用大括号可以输入:字符串,变量,函数以及js对象

## react的遍历

```react
{list.map(item => <li key={item.id}>{item.name}</li>)}
```

## 条件渲染

```react
{flag && <span>这是flag</span>}
{flag ? <span>这是true</span> : <span>这是false</span>}
```

我们也可以把这个封装为一个函数

```react
function getArticle() {
  if (articleType === 1) {
    return <div>文章类型1</div>
  }
  else if (articleType === 2) {
    return <div>文章类型2</div>
  } else {
    return <div>没有文章</div>
  }
}
```

## 事件绑定

```react
const handlerClick = (e) => {
    console.log("我被点击了",e)//这里的e是事件参数
}
<button onClick={handlerClick}>点我</button>

//如果想要自定义传入
const handlerClickJack = (name) => {
	console.log("我被点击了", name)
}
<button onClick={(e) => { handlerClick("jack"); console.log(e) }}>点我2</button>
```

## UseState

数据一旦变化,试图也变化

