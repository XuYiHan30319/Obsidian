```c++
class Account{
	static dobule rate(){return rate;}//static函数只能使用staic变量,没this指针
	stastc int rate;
}

//在类外可直接访问static,不论private或者public.在类外定义的时候,不使用static关键字
double Account::rrate=10;
```

