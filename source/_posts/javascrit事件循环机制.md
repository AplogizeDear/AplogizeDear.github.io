---
layout:
  - javascrit事件循环机制
title: javascrit事件循环机制
date: 2021-08-12 11:46:42
tags: 知识
---

## 8月12号

### JavaScript执行机制

#### 关于javascript

​	Javascript是一门单线程语言，所有的多线程都是通过单线程模拟出来的333

#### javascript 事件循环

1. 同步任务
2. 异步任务

同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。

当指定的事情完成时，Event Table会将这个函数移入Event Queue。

主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。

上述过程会不断重复，也就是常说的Event Loop(事件循环)。

#### setTimeout

```javascript
setTimeout(() => {
    task()
},3000)

sleep(10000000)
```

`setTimeout`这个函数，是经过指定时间后，把要执行的任务(本例中为`task()`)加入到Event Queue中，又因为是单线程任务要一个一个执行，如果前面的任务需要的时间太久，那么只能等着，导致真正的延迟时间远远大于3秒。

`setTimeout(fn,0)`的含义是，指定某个任务在主线程最早可得的空闲时间执行，意思就是不用再等多少秒了，只要主线程执行栈内的同步任务全部执行完成，栈为空就马上执行。即便主线程为空，0毫秒实际上也是达不到的。根据HTML的标准，最低是4毫秒。有兴趣的同学可以自行了解。

#### setInterval

`setInterval`会每隔指定的时间将注册的函数置入Event Queue，如果前面的任务耗时太久，那么同样需要等待。一旦**`setInterval`的回调函数`fn`执行时间超过了延迟时间`ms`，那么就完全看不出来有时间间隔了**。

### Promise与process.nextTick(callback)

宏任务 和 微任务

```javascript
setTimeout(function() {
    console.log('setTimeout');
})
new Promise(function(resolve) {
    console.log('promise');
}).then(function() {
    console.log('then');
})

console.log('console');
```

时间循环机制 先执行宏任务再执行微任务再执行宏任务

```javascript
console.log('1');

setTimeout(function() {   //宏任务 setTimeout1
    console.log('2');
    process.nextTick(function() {  //微任务 process2
        console.log('3');
    })
    new Promise(function(resolve)  
        console.log('4');
        resolve();
    }).then(function() {   //微任务 then2
        console.log('5')
    })
})
process.nextTick(function() {  //微任务 process1   一轮 执行
    console.log('6');
})

new Promise(function(resolve) { 
    console.log('7');
    resolve();
}).then(function() {      //微任务 then1  一轮 执行
    console.log('8')
})

setTimeout(function() {    // 宏任务 setTimeout2
    console.log('9');  
    process.nextTick(function() {   // 微任务process3
        console.log('10');
    })
    new Promise(function(resolve) {  
        console.log('11');
        resolve();
    }).then(function() {   //微任务 then3
        console.log('12')
    })
})
```

|      | 宏任务Event Queue | 微任务Event Queue |
| ---- | ----------------- | ----------------- |
|      | setTimeout1       | process1          |
|      | setTimeout2       | then1             |

第一轮 宏任务   1   7  微任务    6  8

| 宏任务Event Queue | 微任务Event Queue |
| ----------------- | ----------------- |
| setTimeout1       | process2          |
|                   | then2             |

第二轮 宏任务 2  4  微任务 3 5   

| 宏任务Event Queue | 微任务Event Queue |
| ----------------- | ----------------- |
| setTimeout2       | process3          |
|                   | then3             |

第三轮  宏任务 9   11  微任务 10  12

三次循环 1 7 6 8 2 4 3  5 9 11 10 12

### 最后

1. js 是一门单线程语言，所有的异步都是同步模拟的
2. 事件循环是js实现异步的一种方法，也是js的执行机制
3. 执行js因环境不同而不同 运行js是指javascript解析引擎
4. javascript是一门单线程语言
5. Event Loop是javascript的执行机制

​	




