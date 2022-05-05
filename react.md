# react 源码

###代码结构
react：react 核心包，提供主要使用的 api
react-dom：react 渲染器包，与 react-native 平级 web 端渲染
react-reconciler：管理 react 应用状态的输入和结果的输出. 将输入信号最终转换成输出信号传递给渲染器.
scheduler：调度机制的核心实现, 控制由 react-reconciler 送入的回调函数的执行时机, 在 concurrent 模式下可以实现任务分片

###两个循环

- 1.任务调度循环：

  源码位于 Scheduler.js, 它是 react 应用得以运行的保证, 它需要循环调用, 控制所有任务(task)的调度.

- 2.fiber 构造循环:

  源码位于 ReactFiberWorkLoop.js, 控制 fiber 树的构造, 整个过程是一个深度优先遍历.

  这两个循环对应的 js 源码不同于其他闭包(运行时就是闭包), 其中定义的全局变量, 不仅是该作用域的私有变量, 更用于控制 react 应用的执行过程.

#####主干逻辑
1、修改状态，修改 dom，任务发起
2、react-reconciler 注册 task
3、scheduler 任务调度发起执行 task，在 reconciler 中执行 task
执行一：fiber 构造  
 执行二：commitRoot
