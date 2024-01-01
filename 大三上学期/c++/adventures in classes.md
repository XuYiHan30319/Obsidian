# copy construct & assign operator
copy\_construct:class\_name(const classname\&)
```c++
stringBad motto;
stringBad ditto(motto);//class(const classname &)
stringBad also = string(motto);
```
值传递或者返回一个类或者创建一个临时变量的时候的时候都会发生拷贝构造函数
所以我们需要使用深拷贝来对static类或者指针类型做操作.
class_name & classname::operator=(const class_name &);
不过可能会使用拷贝构造函数,看编译器喽
```c++
stringBad::stringBad(const stringBad&st){
	...
}

stringBad& stringBad::operator=(const stringBad&st){
	if(this==&st){
		return *this
	}
	delete []str;
	len = st.len;
	str = new char[n+1];
	std::strcpy(str,st.str);
	return *this;
}
```

# 初始化列表
