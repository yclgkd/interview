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
