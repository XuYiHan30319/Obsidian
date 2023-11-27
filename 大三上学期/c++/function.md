# Advanced functions

[toc]



## 内联函数

inline void nihao(int n){

}

使用内敛函数可以让程序在执行函数的时候不开栈，这样可以大大减少程序的运行开销，但是需要更多的空间

```c++
incline double squre(double x){
  return x*x;
}

#define squre(x) ((x)*(x)
```

## 引用

int &r = rats;

一个引用在初始化之后不能改变

>  实际上引用和*const 是一个意思，就是指针的指向不能变

const引用可以接受字面量，表达式和不同类型的变量

void cube(int &a);这个函数不能这样调用：cube(3);cube(x+1);cube(一个double)

void cuble(const int &a);却都可以，这是因为传参的时候先生成了一个临时变量



## 函数overloading重载

void sink(double &k);//左值

void sink(const double &k);//常左值

void sink(double &&r3);//右值

确定调用哪个函数需要让函数做个差集









