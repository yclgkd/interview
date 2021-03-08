# JavaScript面试题

## 变量类型和计算

### typeof能判断哪些类型

- 所有的值类型：number string undefined boolean Symbol
- 函数和Object（null, object, array）

### 何时使用 == 何时使用 ===  

- null和undefined的时候用==，x == null
- 其他情况用 ===  

### 手写深拷贝

```js
function deepClone(obj = {}) {
    if (typeof obj !== 'object' || obj == null) {
        // obj 是 null ，或者不是对象和数组，直接返回
        return obj
    }

    // 初始化返回结果
    let result
    if (obj instanceof Array) {
        result = []
    } else {
        result = {}
    }

    for (let key in obj) {
        // 保证 key 不是原型的属性
        if (obj.hasOwnProperty(key)) {
            // 递归调用！！！
            result[key] = deepClone(obj[key])
        }
    }

    // 返回结果
    return result
}
```

### JS数据类型有哪些

JavaScript 的类型分为两种：原始数据类型（Primitive data types）和对象类型（Object types）。
原始数据类型包括：布尔值、数值、字符串、null、undefined 以及 ES6 中的新类型 Symbol 和 ES10 中的新类型BigInt。所以一共是八种。

## 异步

### 同步和异步有何不同

- 异步基于JS是单线程语言
- 异步不会阻塞代码执行
- 同步会阻塞代码执行

### 异步的应用场景

- 网络请假，如ajax图片加载
- 定时任务，如setTimeout

### Promise解决了什么问题

解决了callback hell（回调地狱），它是一个管道的形式而不是不断的嵌套。

### Promise的三种状态

- pending resolved rejected
- pending -> resolved 或 pending -> rejected
- 变化不可逆

### Promise状态的表现和变化

- pending状态，不会触发then和catch
- resolved状态，会触发后续的then回调函数
- rejected状态，会触发后续的catch回调函数

### then和catch对状态的影响

- then正常返回resolved，里面有报错则返回rejected
- catch正常返回resolved，里面有报错则返回rejected

### 手写Promise加载一张图片

```js
const url = ''
function loadImg(src) {
    const p = new Promise(
        (resolve, reject) => {
            const img = document.createElement('img')
            img.onload = () => resolve(img)
            img.onerror = () => {
                const error = new Error(`图片加载失败，图片地址：${src}`) 
                reject(error)
            }
            // 一赋值就会触发图片的加载，加载成功执行resolve函数，加载失败执行reject函数
            img.src = src
        }
    )
    return p
}
```

### 请描述event loop（事件循环/事件轮询）的机制，可画图

- 同步代码，一行一行放在Call Stack中执行
- 遇到异步，会先“记录”下，等待实际（定时、网络请求等）
- 时机到了，就移动到Callback Queue中
- 如Call Stack为空（即同步代码执行完），
- 执行当前的微任务
- 尝试DOM渲染
- 然后Event Loop开始工作
- 轮询查找Callback Queue，如有则移动到Call Stack执行
- 然后继续轮询查询（永动机一样）

### DOM事件和event loop

- JS是单线程的
- 异步(setTimeout, ajax等)使用回调，基于event loop
- DOM事件也使用回调，基于event loop
- 它们的触发时机不一样，这里要注意DOM事件不是异步

### async/await和Promise的异同

- 都解决了callback hell的问题
- Promise then catch链式调用，但也是基于回调
- async/await是同步语法，彻底消灭回调函数

### async/await和Promise的关系

- 执行async函数，返回的是Promise对象
- await相当于Promise的then
- try...catch可捕获异常，代替了Promise的catch

### for...of的应用场景

可以用在异步场景，与foreach和for...in...不同，需要等待结束才执行下一个

```js
// 定时算乘法
function multi(num) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(num * num)
        }, 1000)
    })
}

// // 使用 forEach ，是 1s 之后打印出所有结果，即 3 个值是一起被计算出来的
// function test1 () {
//     const nums = [1, 2, 3];
//     nums.forEach(async x => {
//         const res = await multi(x);
//         console.log(res);
//     })
// }
// test1();

// 使用 for...of ，可以让计算挨个串行执行
async function test2 () {
    const nums = [1, 2, 3];
    for (let x of nums) {
        // 在 for...of 循环体的内部，遇到 await 会挨个串行计算
        const res = await multi(x)
        console.log(res)
    }
}
test2()
```

### 什么是宏任务macroTask什么是微任务microTask

- 宏任务：setTimeout, setInterval, Ajax, DOM事件
- 微任务：Promise, async/await
- 微任务要比宏任务执行时机早

### event loop和DOM渲染

- 每次Call stack清空（即每次轮询结束），即同步任务执行完
- 都是DOM重新渲染的机会，DOM结构如有改变则重新渲染
- 然后再去触发下一次的Event Loop

### 宏任务和微任务的区别

- 宏任务：DOM渲染后触发，如setTimeout
- 微任务：DOM渲染前触发，如Promise

## DOM（Document Object Model）

### DOM的本质

DOM的本质是html解析出来的一棵树

### DOM操作的常用API

```js
document.getElementById
document.getElementsByTagName
document.getElementsByClassName
document.querySelector
document.querySelectorAll
```

### attr和property的区别

- property：修改对象属性，不会体现到html结构中，如dom.nodeName dom.style.width dom.style.
- attribute：修改html属性，会改变html结构，如dom.getAttribute dom.setAttribute
- 两者都可能引起DOM的重新渲染

### 一次性插入多个DOM节点，考虑性能

