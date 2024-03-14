

## bean扫描注册

在springboot中扫描到的包范围是启动类所在的包及其子包

- Component
- Controller:用于控制器
- Service:用于业务类
- Repository

**model层**
[model](https://so.csdn.net/so/search?q=model&spm=1001.2101.3001.7020)层级数据库实体层，往往也被称为entity层，pojo层。
一般数据库一张表对应一个实体类，类中属性与表中字段一一对应。
**dao层**
dao层即数据持久层，也被称作mapper层（springboot+mabatis中会用到）。
dao层的作用是访问数据库，向数据库发送[SQL语句](https://so.csdn.net/so/search?q=SQL语句&spm=1001.2101.3001.7020)，完成数据的增删改查。
**service层**
service层即业务逻辑层。
service层的作用是完成功能设计。
service层调用dao层接口，接收dao层返回的数据，完成项目的基本功能设计。
**controller层**
controller层是控制层。
controller层的功能是请求和响应控制。
controller层负责前后端交互，接收前端请求，调用service层，接收service层返回的数据，最后返回具体的页面个数据到客户端





- 首先，在类顶部添加 **@RestController** 注解。
- 现在，编写一个带有 **@RequestMapping** 注解的请求 URI 方法。
- 然后，Request URI 方法应该返回 **Hello World** 字符串。



这里是返回数据的格式

@JsonIgnore

Pojo.Result

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Result<T> {
    private Integer code;
    private String message;
    private T data;

    public static <E> Result<E> success(E data) {
        return new Result<>(0, "操作成功", data);
    }

    public static Result success() {
        return new Result(0, "操作成功", null);
    }

    public static Result error(String message) {
        return new Result(1, message, null);
    }
}
```

Controller->

```java
@RestController
@RequestMapping("/user")
public class UserController {

    @Resource
    private UserService userService;

    @PostMapping("/register")
    public Result register(String username,String password){
        //查询是否存在这个用户
        User u = userService.findUserByName(username);
        if(u==null){
            userService.register(username,password);
            return Result.success();
        }else{
            return Result.error("该用户名被占用");
        }
    }
}
```

Service->

```java
public interface UserService {
    User findUserByName(String username);

    void register(String username, String password);
}
```

service.impl

```java
@Service
public class UserServiceImpl implements UserService {
    @Autowired
    private UserMapper userMapper;

    @Override
    public User findUserByName(String username) {
        User user =  userMapper.findByUserName(username);
        return  user;
    }

    public void register(String username, String password) {
        userMapper.add(username,password);
    }
}

```

Mapper

```java
@Mapper
public interface UserMapper {
    @Select("select * from user where username=#{username}")
    User findByUserName(String username);

    @Insert("INSERT into user(username,password) values(#{username},#{password})")
    void add(String username, String password);
}
```

Exception.globalExceptionHandler 并且在Controller加上@Validated

```java 
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public Result handleExpetion(Exception e){
        e.printStackTrace();
        return Result.error(StringUtils.hasLength(e.getMessage())?e.getMessage():"操作失败");
    }
}
```

![image-20240304160516061](/Users/blackcat/Library/Application Support/typora-user-images/image-20240304160516061.png)

![image-20240304160818882](/Users/blackcat/Library/Application Support/typora-user-images/image-20240304160818882.png)从请求头中的Authorization返回请求

为了统一认证,我们使用拦截器代码

->configure

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Resource
    private LoginInteceptor loginInteceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginInteceptor).excludePathPatterns("/user/login","user/register");
    }
}

```

->Interceptor

```java
@Component
public class LoginInteceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String token =  request.getHeader("Authorization");
        if(token == "jwt"){
            return true;
        }
        response.setStatus(401);
        return false;
    }
}
```


```mermaid
graph TD;
    根节点((开始)) --> A;
    根节点 --> I;
    根节点 --> M;

   subgraph 数学题解答
        A[生成数学应用题] --> B[解析数学题文本];
        B --> C[使用爬虫在百度网站爬取对应的图片];
        B --> D[使用文本转语音 API得到语音];
        C --> E[生成数学题图片];
        D --> F[生成语音];
        F-->KK[abobe根据语音和角色生成视频]
        E --> G[视频编辑];
        KK --> G;
        G --> H[生成解答视频];
    end

    subgraph 绘画
        I[接收对象和画作];
        I --> J[使用预训练模型进行编码解码采样];
        J --> K[生成图片];
    end

    subgraph 古诗赏析
        M[拆解古诗];
        M --> N[生成每句诗句的对应视频];
        N --> O[使用大语言模型赏析];
        O --> P[拼接视频];
    end
    H-->EN
    K-->EN
    P-->EN
    EN((返回前端))
    config::width: 100%;
```
