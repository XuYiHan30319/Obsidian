utils/request.js

```react
// axios的封装处理
import axios from "axios"
import { getToken, removeToken } from "./token"
import router from "@/router"
// 1. 根域名配置
// 2. 超时时间
// 3. 请求拦截器 / 响应拦截器

const request = axios.create({
    baseURL: 'http://geek.itheima.net/v1_0',
    timeout: 5000
})

// 添加请求拦截器
// 在请求发送之前 做拦截 插入一些自定义的配置 [参数的处理]
request.interceptors.request.use((config) => {
    // 操作这个config 注入token数据
    // 1. 获取到token
    // 2. 按照后端的格式要求做token拼接
    const token = getToken()
    if (token) {
        config.headers.Authorization = `Bearer ${token}`
    }
    return config
}, (error) => {
    return Promise.reject(error)
})

// 添加响应拦截器
// 在响应返回到客户端之前 做拦截 重点处理返回的数据
request.interceptors.response.use((response) => {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response.data
}, (error) => {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    // 监控401 token失效
    console.dir(error)
    if (error.response.status === 401) {
        removeToken()
        router.navigate('/login')
        window.location.reload()
    }
    return Promise.reject(error)
})

export { request }

```

token持久化:localStorge.setItem("token_key",action.payload),然后token的initialstate是localStorge.getItem("token_key")||''

->components/authroute

```react
import { Navigate } from "react-router-dom";
import { getToken } from "../utils/token";

export function AuthRoute({ children }) {
    const token = getToken()
    if (token) {
        return <>{children}</>
    } else {
        return <Navigate to={"/login"} replace></Navigate>
    }
}
```

->router/index.js

```react
const router = createBrowserRouter([
    {
        path: "/login",
        element: <Login />
    },
    {
        path: "/",
        element: <AuthRoute><Layout /></AuthRoute>//在这里包裹一层就可以防止登录了
    }
])
```

## 模块封装

比如  const res = await request.post('/authorizations', loginForm)写成

```react
{/*apis中的user.js*/}

export function loginAPI(formData){
  return request({
    url:"/login",
    method:"POST",
    data:formData,
  })
}

//上面的替换为
 const res = await loginAPI(formData)
```

