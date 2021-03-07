# JavaScript面试题

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
- 如Call Stack为空（即同步代码执行完）Event Loop开始工作
- 轮询查找Callback Queue，如有则移动到Call Stack执行
- 然后继续轮询查询（永动机一样）

### DOM事件和event loop

- JS是单线程的
- 异步(setTimeout, ajax等)使用回调，基于event loop
- DOM事件也使用回调，基于event loop
- 它们的触发时机不一样，这里要注意DOM事件不是异步
