# 什么是css

## css导入方式

- 直接卸载style

- 直接在属性写,这种优先级更高,会覆盖外部样式,用于动态样式或者临时样式
- 直接外部链接(推荐,可以分离)使用link,该方法更加优先
- 使用@import,等页面加载完毕再加载,页面会闪烁

## css具体写法

```css 
p {
	color:red,
	width:300px,
}

a,b,c {
  color:red,
}

hi{}//标签选择器
.box {}//类选择器 class="box",更常用,因为不用保证不重复
#unique{} // id选择器 id='unique'
a[title] {}//所有带有title的<a>元素

<style>
    .d1:hover {
        background-color: red;
        color: green
    }


    a:hover {
        background-color: #66ccff;
    }

    a:visited {//伪类
        color: green;
        background-color: red;
    }

	 .p1::first-letter {//伪元素
          color: red
    }

	//组合选择器
  
	div p {
    //选择div中的所有p元素
  }
	
	
</style>

<div class="d1">
    点我
</div>
```

css优先级,以下优先级递减

- 加入了!important
- 行内html
- 外部和内部样式表(head里)
- 默认样式



border-style的顺序:上右下左

















