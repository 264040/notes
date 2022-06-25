# CSS：font-wieght（粗细设置）

```css
normal	默认值。定义标准的字符。 
a{
  font-wieght: normal;
}

bold	定义粗体字符。
a{
  font-wieght: bold;
}

bolder	定义更粗的字符。
a{
  font-wieght: bolder;
}

lighter	定义更细的字符。
100,200,300,400,500,600,700,800,900
a{
  font-wieght: 100;
}

定义由粗到细的字符。400 等同于 normal，而 700 等同于 bold。
inherit	规定应该从父元素继承字体的粗细。
a{
  font-wieght: inherit;
}
```

# CSS：文本换行显示/文本超出设成省略号

```css
overflow: hidden;	// 超出隐藏
text-overflow: ellipsis;	// 文字超出宽度设置成省略号 参数：clip ellipsis 
white-space: nowrap;   //  文字不换行显示（ 默认值normal（换行））
```

# CSS：button默认样式

```css
.btn {
  outline: none; /*消除默认点击蓝色边框效果*/
  border: none;
  cursor: pointer; /* 鼠标样式 */
}
```

# CSS：清除浮动clear: buth

```css
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .divcss5 {
      width: 400px;
      border: 1px solid #F00;
      background: #FF0;  
    }
    .divcss5::after{	// 伪元素清除浮动
      content: "";
      clear: both;
      display: inline-block;
      visibility: hidden;		// 隐藏伪元素内容content
    }
    .divcss5-left,
    .divcss5-right {
      width: 180px;
      height: 100px;
      border: 1px solid #00F;
      background: #FFF
    }

    .divcss5-left {
      float: left; 
    }

    .divcss5-right {
      float: right; 
    }
 
  </style>
</head>

<body>
  <div class="divcss5">
    <div class="divcss5-left">left浮动</div>
    <div class="divcss5-right">right浮动</div> 
  </div>
</body>

</html>
```

