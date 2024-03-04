# router

```react
bun i react-router-dom
```

```react
const router = createBrowserRouter([
  {
    path: '/login',
    element: <div>我是登录</div>
  },
  {
    path: '/article',
    element: <div>我是文章</div>
  },
])

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <RouterProvider router={router}>
    </RouterProvider>
  </React.StrictMode>
);
```

↑最简单的方法

.
├── App.jsx
├── index.js
├── page
│   ├── article
│   │   └── index.js
│   └── login
│       └── artile.js
└── router
    └── index.js

优化之后

```react
const router = createBrowserRouter([
    {
        path: "/login",
        element: <Login />
    },
    {
        path: "/article",
        element: <Artile />
    }
])

export default router
```

## 路由导航

### 声明式导航

比如左侧导航栏:<link to="/aritle">

​            <Link to="/article">点我跳转</Link>

### 编程导航

```react
function Login() {
    const navigate = useNavigate()
    return (
      	<div>
            <Link to="/article">点我跳转</Link>
            我是登录
            <button onClick={() => { navigate("/article") }}>登录</button>
        </div>
    )
}
```

### 传参

```react
const [parama] = useSearchParams()//得到url中的内容,针对/article?id=100&name=jack
const id = parama.get("id")
const name = parama.get("name")

//针对/article/100/jack
const router = createBrowserRouter([
    {
        path: "/login",
        element: <Login />
    },
    {
        path: "/article/:id/:name",
        element: <Artile />
    },
  {/*404路由配置*/}
    {//需要放在最后,是兜底组件
      path:"*",
      element:<NotFound/>
    }
])
const params = useParams()
const id = params.id
const name = params.name
```

### 嵌套路由

![image-20240301181919234](/Users/blackcat/Library/Application Support/typora-user-images/image-20240301181919234.png)

```react
const Layout = () => {
    return (
        <div>
            我是一级路由
            <Link to="/board">面板</Link>
            <Link to="/about">关于</Link>
            {/* 二级路由出口,下面是固定写法,是渲染的位置 */}
            <Outlet />
        </div>
    )
}
{/*在router中这样修改就行*/}
{
        path: "/",
        element: <Layout />,
        children: [
            {
                index: true//这个是默认二级路由,在默认情况下显示
                element: <Board />
            },
            {
                path: "about",
                element: <About />
            }
        ]
    }



```

### 获得当前的路径

```jsx
const location =  useLocation()
console.log(location.pathname)
```

