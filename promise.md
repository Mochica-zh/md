Promise 是ES6提出的异步编程解决方案，将异步代码用同步的写法表达出来，避免回调地狱
可以查看Promise的静态方法：all、allSettled、race、reject、resolve、any
可以查看Promise的原型对象方法：then 、catch、finally
 Promise的使用：
    - 实例化
    - 参数是一个函数,函数的参数是两个回调函数 resolve reject
    - 当promise中的异步代码执行成功，我们就调用resolve方法，如果执行失败则调用reject方法
Promise实例化得到一个promise对象
    - promise对象中有两个属性PromiseState，PromiseResult
    - PromiseState是当前promise对象的状态
    - PromiseResult是当前promise返回值，promise成功，则拿到的是resolve方法的参数，promise失败则拿到的是reject的参数，其他状态是undefined
Promise的三种状态:
    - pending:正在执行中，代表promise内部还没有调用成功或失败函数
    - fulfille/resolved:代表promise是成功状态
    - rejected:代表promise是失败状态
promise是处理异步代码的，但是promise内的代码执行是同步的

拿到数据以后不要直接操作数据 而是调用resolve方法 把数据和状态返回出去 再使用
避免了逻辑嵌套过深
const p1 = new Promise((resolve, reject) => {})
then方法
    - 当promise对象返回成功或者失败的时候就会调用,then其实也是一个异步代码
- 用法1：
    - then方法中书写两个函数,第一个函数是成功处理回调函数，第二个函数的失败处理回调函数
    - then方法中函数的参数 就是成功或失败的返回promise对象的值[PromiseResult]
- 用法2(常用):
    - then中只书写一个函数，代表成功函数，失败的状态由catch接受，then不再负责
    - 目的是为了区分开成功和失败的逻辑
then返回失败的情况：1.失败的promise调用then 2.then中报错了 3.then返回了一个失败的promise对象
then方法的返回值：
    - then的返回值一定是一个promise对象
    - 默认没有执行到then，只是定义了then的时候，then的返回值是一个pending状态的promise对象
    - 当then中没有书写返回值,then的返回值是一个fulfilled成功状态的promise对象，promise状态的值是undefined
    - 当then中书写了return一个不是promise对象的值，那么then方法的返回值是fulfilled成功状态，值是return的内容
    - 当then中的代码出现了错误，则直接返回rejected失败状态promise对象，并且值是错误信息
    - 当then中返回的是一个promise 对象，则then的返回值是这个promise对象
    - 当调用then方法的promise对象是失败的，则then不会执行自己的内容，而是直接返回一个失败的promise对象，里边的值就是刚才失败promise对象的值

    当一个失败的promise对象调用catch方法的话，就会进去catch执行
        当一个失败的promise对象调用then方法的时候，then方法不会执行里边的内容，而是直接返回一个失败的promise对象
        catch返回值的规则和then一样
    无论是失败的还是成功的promise对象 都可以调用finally方法
        一般finally方法会放在promise调用的最后一步（不再调用了，所以不考虑返回值）
        finally的回调函数一般不书写参数，只是为了做最后一步操作

Promise.all:
    - 可以接收多个promise实例化对象
    - 当all中的promise对象没有执行完成并且没有失败的promise对象，则返回值一直是一个peding状态
    - 如果有一个失败了，就不会向下继续请求了，直接返回失败的promise对象，则返回一个失败的promise对象，他的值就是其中失败的那个promise对象的值
    - 当参数的promise对象都成功，则返回成功promise对象
    - 其中 promise对象的状态是fulfilled promise对象的值是一个数组，分别是所有成功promise对象的值
Promise.allsettled:
    - 可以接收多个promise实例化对象
    - 无论allsettled中所有的promise对象成功还是失败，都会返回一个成功的promise对象
    - 返回的promise对象的值是一个数组，数组中是所有promise实例化对象执行完成后的状态和值
Promise.race:
    - 返回一个promise对象
    - 无论race中的promise对象最先的那个返回的失败还是成功，都会停止执行，返回当前最先完成的promise对象
Promise.any:
    - 返回一个promise对象
    - 只要any中有一个promise对象返回成功状态，则any就返回这个成功状态的promise对象
    - 如果any中所有的promise对象失败，则返回一个失败的promise对象，值是一个错误AggregateError: All promises were rejected

async把一个函数声明为了一个异步函数，配合await使用
    async返回一个promise对象
    默认情况下 async函数返回一个成功的promise对象，promise对象的值是 函数的返回值
    为什么async会返回一个promise对象呢？
    async函数 通过Promise.resolve()方法把 函数的返回值封装并返回一个promise对象

    - await 需要在async函数中书写，代表等待的意思
    - await 能够真正等待的是promise对象 其他的一概不等
    - 只有await等待的promise成功或者失败，才会进行下一个动作，如果是pending状态，则一直等待不动
    await返回值：
        - 如果await等待的是一个成功的promise对象，则awiat的返回值就是这个promise对象的值
        - 如果await等待到的是一个失败的promise对象，则直接退出async函数，async函数会返回一个失败的promise对象
        - 如果await等待的不是一个promise对象，则await的返回值就是await等待的值
        - 如果await等待的promise对象没有失败，则async的返回值将会一直是成功状态的promise对象，值是函数return的值
