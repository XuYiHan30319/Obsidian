在html中,JavaScript的代码写在标签\<script>中间
当然我们也可行做外部js,比如在js文件夹下放了个my.js文件,那么我们就可以用\<script src="路径">来引入,如果引入了内容,那么就不能再插入内容了,否则会被忽略
可以在行内,嵌入和外部
```html
<input type="button" onclick="alert('helloword')">

<script>
	alert('helloworld')
<\script>
```
在js中通常使用单引号,html中用双引号
# js输入输出语句
prompt:浏览器弹出输入框
alert:浏览器弹出警示框
console.log()//日志显示
# js 变量
比如想接收用户输入的数据var x = prompt('123'),就需要用到变量
声明变量:var x//没有符号类型
如果没有初始化,那么值是undefined
当然,和py一样,可以不声明就是用变量
js变量的数据类型是动态的,和py一样,在运行过程中才会被确认
## js数据类型
Number:所有的数字型,Number.MAX_VALUE就是无穷大
	isNaN(x)判断是不是数字类型,是数字返回false,否则true
boolean:true 和 false
String:字符型
	str.length可以获得字符串长度,不需要加括号
	str1+str2是拼接
	str1+Number也是字符串类型拼接
Null:空
比如我们有个var x = undefined,那么x+'123'=undefined123
检测数据类型:typeof x(注意这里不需要括号)
## 数据类型转换
转换为字符串:函数toString(),强制类型转换String(),字符串拼接123+''
转换为数字:parseInt(string) parseFloat(string),Number()强制类型转换,隐式类型转换'12'-0结果是12
\==号会自动转换类型,\=\==三个等号要求符号类型也相等
# 流程控制语句

## if
和c语言一样
if(){

}
else if(){

}
else{

}
## while
while(){

}
## for
for(var i=0;i<100;i++){

}

# 数组
var arr = new Array();创建一个空数组
var arr = [1,2,'213',true];//可以随便初始化
数组下标越界显示的是undefined
获取数组长度可以直接arr.length
arr.length = 5;修改数组长度为5
arr[i] = "123";//这样也可以
# 函数
function fun(){
	console.log('你好')
}

function fun(x){
//和py一样,函数调用的时候不需要输入类型
}
# 对象
var obj  = {
	uname:"123",
	age:"123"
	sayhi:sayHi(){
		...
	}
}

var ob = new object();
ob.name  = ...
ob.sayhi() = function(){
					...
					}
## 类构造函数
function star(user,name,age){
		this.user = user;
		this.name = name;
		this.age = age;
}
new star('123',213,123)

