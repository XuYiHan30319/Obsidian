# 什么是HTML

html以<!DOCTPE html>开头,告诉浏览器用的哪个html版本这里表示HTML5

第二行以<head>开头,定义一些元数据什么的

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

p段落标签,br表示换行,一般在br后面加上/表示自闭和 hr/ 表示打一条横线	

div表示一块

span表示行内元素不会换行

ul表是unorder list,相当于MarkDown的- 和 1. 

- 1
- 2
- 3

1. 1
2. 2
3. 3

```html
	<ul>
		<li>1</li>
		<li>2</li>
		<li>3</li>
		<li>4</li>
		<li>5</li>
	</ul>

	<ol>
		<li>1</li>
		<li>2</li>
		<li>3</li>
		<li>4</li>
		<li>5</li>
	</ol>
```

表格

```html
	<table>
		<tr>
			<th>姓名</th>
			<th>体重</th>
		</tr>

		<tr>
			<th>肉丸</th>
			<th>1000kg</th>
		</tr>
	</table>
	
```

输入:

```html
<form>
  <label>username:</label>
  <input placeholder="我草" type="text" username="username" id="username">
</form>
```

jpg有损压缩体积更小,支持渐进式加载,但是不支持透明盒半透明

GIF支持动画,无损,压缩比大

PNG支持透明背景但是文件大,不支持动画

webp很好哦结合png和jpg优点,但是需要兼容性处理

## HTML文档结构

DOM树(Doucment Object Model)

CSSOM树

在head头放入css资源,随着文档流解析文件

在文件底部引入js资源,因为js会阻塞html的引入,造成页面空白和假死,所以引入了async和defer,async执行与文档顺序无关,先加载就先执行那个,加载完立刻执行,defer等到所有文档元素下载完再执行,区别就是执行时间



