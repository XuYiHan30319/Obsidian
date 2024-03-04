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

## useState 和 useRef

```react
const myRef = useRef(null)
const [text,setText] = useState("")
<input ref={myRef} onChange={(e)=>{setText(e.target.value)}} value={text} type="text"/>
<button onclick={()=>{myRef.current.focus();setText("");}}>点我<button/> 
```

## 组件通信

- 父传子通信:父组件传递数据,子组件身上接受数据 

  ```react
  function Son(props) {
    console.log(props)
    return <div>this is son{props.name}{props.Children}</div>
  }
  
  function App() {
    const name = "this is name";
  
    return (
      <div className="App">
  		  {/*这里是父组件向孩子传递信息*/} 
       <Son name={name} >
         {/*这里是一个特殊的props,会多一个children元素*/}
          <span>你好</span>
        </Son>
      </div >
    );
  }
  ```

  使用props可以进行通信捏,啥都可以传递,包括对象,jsx等,但是子组件不能修改他,不然会爆错

- 子传父:子组件调用父组件的函数

```react
function Son(props) {
  return <>
    <div>this is son{props.name}{props.Children}</div>
    <button onClick={() => { props.onGetMeg("我的"); }}>点我</button>
  </>
}

function App() {
  const name = "this is name";
  const getMessage = (msg) => { console.log(msg) }
  return (
    <div className="App">
      <Son name={name} onGetMeg={getMessage}>
        <span>你好</span>
      </Son>
    </div >
  );
}
```

- 兄弟之间传递:调用共同父组件的方法

```react

function A({ getName }) {
  return <div>
    我是a
    <button onClick={() => { getName("A") }}>点我</button>
  </div>
}

function B({ name }) {
  return <div>
    我是b{name}
  </div>
}

function App() {
  const getName = (name) => {
    console.log(name);
    setName(name);
  }

  const [name, setName] = useState("");
  return (
    <div className="App">
      <A getName={getName}></A>
      <B name={name}></B>
    </div >
  );
}
```

- 不同层级之间的传递

```react
const ctx = createContext();

function A() {
  return <div>
    我是a
    <button>点我</button>
    <B />
  </div>
}

function B() {
  const msg = useContext(ctx);
  return <div>
    我是b{msg}
  </div>
}

function App() {
  const msg = "msg";
  return (
    <div className="App">
      <ctx.Provider value={msg}>
        this is app
        <A ></A>
      </ctx.Provider>
    </div >
  );
}

```

## useEffective

一渲染完毕就向后端请求数据,由渲染引起的操作

useEffective(()=>{},[可选,指定前面的执行时间])

```react
function App() {
  const [list, setList] = useState([])

 useEffect(() => {
    async function getList() {
      const res = await fetch(URL);
      const list = await res.json();
      console.log(list)
      setList(list.data.channels);
    }
    getList();
    const timer = setInterval(() => {
      console.log("我草");
    }, 1000);
    return () => {//在组件清除的时候关闭
      clearInterval(timer);
    }
  }, [])//空数组的时候只会执行一次,不加的话组件更新执行一次,传入特定依赖比如list,当他变化的时候执行

  return (
    <div className="App">
      <ul>
        {list.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>
    </div >
  );
}

```

## 自定义HOOK

比如想要封装一个公有逻辑,可以把代码封装起来,比如下面的代码实现了点击显示/隐藏,我们把这个逻辑封装

```react
function App() {
  const [value, setValue] = useState(true);

  const click = () => {
    setValue(!value);
  }
  return (
    <div className="App">
      {value && <div>点我</div>}
      <button onClick={click}>点我</button>
    </div >
  );
}
```

->一般函数名叫做useXxxx()

```react
function useTogle() {
  const [value, setValue] = useState(true);

  const click = () => {
    setValue(!value);
  }
  return { value, click };
}

function App() {
  const { value, click } = useTogle();

  return (
    <div className="App">
      {value && <div>点我</div>}
      <button onClick={click}>点我</button>
    </div >
  );
}
```













 
