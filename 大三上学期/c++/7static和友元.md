```c++
class Account{
	static dobule rate(){return rate;}//static函数只能使用staic变量,没this指针
	stastc int rate;
}

//在类外可直接访问static,不论private或者public.
class MyClass {
public:
    MyClass(int a = myStaticInt) {}//可以作为默认参数
    static int myStaticInt; // 声明一个静态类成员
    static MyClass cc;//也可以的,但是非static不可以

};

// 定义和初始化静态类成员
int MyClass::myStaticInt = 0;
```



