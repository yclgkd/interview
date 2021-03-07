# CSS面试题

## 盒模型的宽度如何计算

答：offsetWidth = （内容宽度 + 内边距 + 边框），无外边距

- box-sizing: content-box，标准盒模型，offsetWidth = width + padding + border
- box-sizing: border-box，IE盒模型，offsetWidth = width

## margin纵向重叠的问题

- 相邻元素的margin-top和margin-bottom会发生重叠
- 空白内容的`<p></p>`也会发生重叠

## margin负值的问题

- margin-top和margin-left设置为负值元素会向上向左移动
- margin-right负值，自身不会受到影响，右侧元素左移，在外界看来就是自身的宽度在缩小
- margin-bottom负值，自身不会受到影响，下方元素上移，在外界看来就是自身的高度在缩小

## BFC的理解和应用

什么是BFC：

- Block format context：块级格式化上下文
- 一块独立的渲染区域，内部元素的渲染不会影响到边界以外的元素

形成BFC的条件：

- float不是none
- overflow不是visible
- position是absolute或fixed
- display是flex inline-block等

BFC的常见应用：

- 清除浮动

## float布局的问题，以及clearfix

### float布局

#### 圣杯布局和双飞翼布局的目的

- 三栏布局，中间一栏最先加载和渲染（内容最重要）
- 两侧内容固定，中间内容随宽度自适应
- 一般用于PC网页

#### 圣杯布局和双飞翼布局的技术总结

- 使用float布局
- 两侧使用margin负值，以便和中间内容横向重叠
- 防止中间内容被两侧覆盖，一个用padding一个用margin

#### 圣杯布局

[在线演示地址](https://codepen.io/yclgkd/pen/YzpORVB)

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>圣杯布局</title>
    <style type="text/css">
        body {
            min-width: 550px;
        }
        #header {
            text-align: center;
            background-color: #f1f1f1;
        }

        #container {
            padding-left: 200px;
            padding-right: 150px;
        }
        #container .column {
            float: left;
        }

        #center {
            background-color: #ccc;
            width: 100%;
        }
        #left {
            position: relative;
            background-color: yellow;
            width: 200px;
            /* 自身移动了宽度的100%相当于，移动到和center重合 */
            margin-left: -100%; 
            /* 再相对于自身向左移动200px */
            right: 200px;
        }
        #right {
            background-color: red;
            width: 150px;
            /* 对自身没有影响，但是右侧元素移动150px，在外界看来就是宽度在缩小，自身宽度是0 */
            margin-right: -150px;
        }

        #footer {
            text-align: center;
            background-color: #f1f1f1;
        }

        /* 手写 clearfix */
        .clearfix:after {
            content: '';
            display: table;
            clear: both;
        }
    </style>
</head>
<body>
    <div id="header">this is header</div>
    <div id="container" class="clearfix">
        <div id="center" class="column">this is center</div>
        <div id="left" class="column">this is left</div>
        <div id="right" class="column">this is right</div>
    </div>
    <div id="footer">this is footer</div>
</body>
</html>
```

#### 双飞翼布局

[在线演示地址](https://codepen.io/yclgkd/pen/xxRaQpJ)

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>双飞翼布局</title>
    <style type="text/css">
        body {
            min-width: 550px;
        }
        .col {
            float: left;
        }

        #main {
            width: 100%;
            height: 200px;
            background-color: #ccc;
        }
        #main-wrap {
            margin: 0 190px 0 190px;
        }

        #left {
            width: 190px;
            height: 200px;
            background-color: #0000FF;
            margin-left: -100%;
        }
        #right {
            width: 190px;
            height: 200px;
            background-color: #FF0000;
            margin-left: -190px;
        }
    </style>
</head>
<body>
    <div id="main" class="col">
        <div id="main-wrap">
            this is main
        </div>
    </div>
    <div id="left" class="col">
        this is left
    </div>
    <div id="right" class="col">
        this is right
    </div>
</body>
</html>
```

### clearfix清除浮动

```css
.clearfix:after {
    content: '';
    display: table;
    clear: both;
}

.clearfix {
    *zoom: 1;
}
```

## flex画色子

### 常用语法回顾

- flex-direction
- justify-content
- align-items
- flex-wrap
- align-self（对于子元素）

### 具体实现

[在线演示地址](https://codepen.io/yclgkd/pen/qBqMLae)

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>flex 画骰子</title>
    <style type="text/css">
        .box {
            width: 200px;
            height: 200px;
            border: 2px solid #ccc;
            border-radius: 10px;
            padding: 20px;

            display: flex;
            justify-content: space-between;
        }
        .item {
            display: block;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: #666;
        }
        .item:nth-child(2) {
            align-self: center;
        }
        .item:nth-child(3) {
            align-self: flex-end;
        }

    </style>
</head>
<body>
    <div class="box">
        <span class="item"></span>
        <span class="item"></span>
        <span class="item"></span>
    </div>
</body>
</html>
```

## absolute和relative分别依据什么来定位？

- relative依据自身定位
- absolute依据最近一层定位元素（absolute、relative、fixed）进行定位，如果都没找到的话就依据body定位

## 居中对齐有哪些实现方式

### 水平居中

- inline元素：text-align: center
- block元素: margin: auto;
- absolute元素：left: 50% + margin-left负值（自身宽度的一半），需要知道子元素的宽
- absolute元素：left: 50% + transform: translate(-50%, 0);

### 垂直居中

- inline元素：line-height的值等于height
- absolute元素：top: 50% + margin-top负值（自身高度的一半），需要知道子元素的高
- absolute元素：top: 50% + transform: translate(0, -50%);
- absolute元素：top,left,bottom,right都设置成0 + margin: auto

## line-height的继承问题

- 父元素写具体的值，如30px，则继承该值
- 父元素写比例，如2/1.5，则继承改比例
- 父元素写百分比，如200%，则继承计算出来的值

## rem是什么？

- px，绝对长度单位，最常用
- em，相对长度单位，相对于父元素
- rem，相对长度单位，相对于根元素，常用于响应式布局（这里的r是root的意思）

### 如何使用

```css
html {
    font-size: 100px;
}
```

对html设置font-size，之后1rem = 100px，0.1rem = 10px

### rem的弊端

- 具有阶梯型，媒体查询跨度比较大

## 网页视口尺寸

- window.screen.height // 屏幕高度
- window.innerHeight // 网页视口高度
- document.body.clientHeight // body高度

## vw/vh、vmax/vmin

- vh：网页视口高度1/100
- vw：网页视口宽的1/100
- vmax：取两者最大值
- vmin：取两者最小值

## 响应式布局的常见方案

- media-query，根据不同的屏幕宽度设置根元素font-size
- rem，基于根元素的相对长度单位进行计算
- vw/vh
