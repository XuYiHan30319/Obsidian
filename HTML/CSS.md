# 概述
![[Pasted image 20230504142550.png]]
## 标签选择器
一条css语句的模板大致如上
标签选择器:就是标签名+{...},为页面中所有的相同标签标注颜色,但是不能差异化设置
```css
<style>
a{//所有的a标签都会变成这种样式
	color:red;
	background-color:blue;
}
</style>
```
## 类选择器
```html
<style>
.red{
	color:red;
}
</style>


<div class="red">xxxxx</div>
```
注意red前面有点但是下面的class没有
## 多类名
一个标签后面的class可以不止一个,中间用空格分隔
```html
<style>
	.red{
		color:red;
	}
	.fo{
		font-size:26px;
	}
</style>

<p class="red fo">123</p>
```
## id选择器
![[Pasted image 20230504145619.png]]
```html
<style>
	#nav{
		color:pink;
	}
</style>
```
使用方式基本和class相同,但是就是id不一样,而且只能调用一次,别人不能使用,有人用了id,其他就不允许使用了,一次性的
## 通配符选择器
用星号表示,能够选择所有的标签
```html
<style>
	*{
		color:red;
	}
</style>
```
# 字体样式大全
## 字体
font-family:"微软雅黑","times","...."可以同时使用多个字体
font-size:26px(26像素),可以给body设置字体大小,这样所有都会设置一样的字体大小了
font-weight设置字体粗细
font-style文字风格,比如是不是斜体的
为了简约空间,我们可以![[Pasted image 20230504151410.png]]
直接这样写
## 文本对齐
只能设置水平对齐方式
text-align:center/left/right;
## 装饰文本
text-decoration![[Pasted image 20230504151956.png]]
首行缩进:text-inden:20px
行间距line-height
# css引入方式
内部样式表:就是\<style>包裹的,理论上可以放在任何地方,但是一般放在head里
行内样式表:用于修改简单样式,\<p style="....">...\<\p>
外部样式表:先自己写一个.css后缀文件,比如style.css,然后使用link标签引入\<link rel="stylesheet" herf="style.css">
## 后代选择器
ol li {

}
表示选择所有ol里面的li的样式进行修改,只要包含在ol里面的li样式全部修改
