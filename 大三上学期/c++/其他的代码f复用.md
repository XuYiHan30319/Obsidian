# Reusing code in c++

[toc]

Valarray(数值的数组)

```c++
Student(const char * str, const double * pd, int n)
                        : name(str), scores(pd, n) {    }

```

列表初始化实际上是string name(str)这样的形式，而不是使用=

初始化顺序不是list的顺序，而是申明的顺序

## private继承(屁用么有）

基类只能有一个

```c++
class student : private string, private valarray<double>//每个都加，默认为私有
{
public:
    void kk()
    {
        string::size();
    }
 	//想要访问name只能这样干   
  	const string &name()
    {
        return (const string &)*this;
    }
};

int main()
{
    student s;
    s.kk();
}
```

<img src="/Users/blackcat/Documents/北交大软件学院许一涵学习资料/大三上学期/c++/其他的代码f复用.assets/image-20231202191520884.png" alt="image-20231202191520884" style="zoom:50%;" />

继承的机制大致如上，主打一个取继承方式和成员的最低。



## 多继承

在多继承中，我们必须给每一个父类都打上继承的方式，否则就是private的（默认）

![image-20231202191739906](/Users/blackcat/Documents/北交大软件学院许一涵学习资料/大三上学期/c++/其他的代码f复用.assets/image-20231202191739906.png)

```c++
class Singer:public Worker{
}

class Waiter:public Worker{
}

class SingingWaiter:public Singer,public Waiter{
}

int main(){
  SingerWaiter obj;
  Singer *s = &obj;//可
  Worker *s = &obj;//错，指向基类的指针变得ambiguous
}
```

使用多继承之后会导致其中出现两个worker，因此我们需要使用虚继承

```c++
class Singer:virtual public Worker{
}

class Waiter:public virtual Worker{
}

class SingingWaiter:public Singer,public Waiter{
}

//使用虚继承之后
int main(){
  SingerWaiter obj;
  Worker *s = &obj;
}
```

<img src="/Users/blackcat/Documents/北交大软件学院许一涵学习资料/大三上学期/c++/其他的代码f复用.assets/image-20231202192123717.png" alt="image-20231202192123717" style="zoom:50%;" />

一起来看看会使用哪个吧

![image-20231202193521839](/Users/blackcat/Documents/北交大软件学院许一涵学习资料/大三上学期/c++/其他的代码f复用.assets/image-20231202193521839.png)

这里使用了虚继承,那么只有一个worker,所以pw->show()输出worker.而singer报错,因为指针指向的是其中基类的函数

![image-20231202193656640](/Users/blackcat/Documents/北交大软件学院许一涵学习资料/大三上学期/c++/其他的代码f复用.assets/image-20231202193656640.png)

从指针角度来看更容易理解

![image-20231202193333436](/Users/blackcat/Documents/北交大软件学院许一涵学习资料/大三上学期/c++/其他的代码f复用.assets/image-20231202193333436-1516813.png)

在这个图片中,因为我们使用了worker::vitual show,那么pw->show(),注意了,这里的worker使用virtual后,worker的子类和孙子类都会起效果

