## Async & Await

> Create Date ：2022/11/14
>
> Author ： Hongyu Chen
>
> Last Change ：2022/11/14
>
> Mail ：2314727037@qq.com





[Promise和Async/Await用法整理 - 简书 (jianshu.com)](https://www.jianshu.com/p/fe0159f8beb4)

[async/await - 简书 (jianshu.com)](https://www.jianshu.com/p/fb1da22f335d)

[async/await 的理解和用法_前端之神的博客-CSDN博客_async/await](https://blog.csdn.net/qq_42941302/article/details/109245356)

之前学习前端时，学习了 Axios 和 Promise。Axios 使用了 Promise语法，进行异步的请求。

Promise多用于异步操作，当要使程序中的异步操作完成后再执行接下来的代码时，就可以使用 `Promise` 来”保证“异步操作的顺序不被打乱。

然而 Promise 链式需要使用太多的 `then` ，所以大伙又基于 Promise 创造了一个进阶版：Async & Await。这相当是对 Promise 进行了一次更加彻底的封装，让大伙用的更爽。