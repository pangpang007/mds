- #### this 上下文对象

  绝大多数情况下，函数的调用方式决定了 this 的值（运行时绑定）。
  this 不能在执行期间被赋值，并且在每次函数被调用时 this 的值也可能会不同。
  [[全局上下文]]：浏览器环境下是 window 严格模式下是 undefined node 环境下是 globalThis
  [[函数上下文]]：在函数内部，this 的值取决于函数被调用的方式
  [[类上下文]]：在类的构造函数中，this 是一个常规对象。类中所有非静态的方法都会被添加到 this 的原型中,箭头函数会添加到 this 常规对象中

  bind 函数可以调用多次，但只能支持修改一次 this 指向

- #### new 运算符

  new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。

  实现过程：

  ```
      1.创建一个javascript空对象{}
      2.为空对象的__proto__属性赋值，目标构造函数中的prototype
      3.绑定上下文this对象，将构造函数的this指向空对象。
      4.如果没有返回值，返回this对象。
      5.如果有返回值,判断返回值。如果返回的是一个对象，则返回该对象，
        如果返回的是其他类型，则返回this对象
  ```

  实现一个 new 关键字

  ```
      function myNew(Obj){
          var empty = {};
          empty.__proto__ = Obj.prototype
          var result = Obj.call(this)
          return result
      }
  ```

- #### 继承和原型链

  与其他基于类的语言相比，js 中没有 class 的概念，只有对象。
  对象中的私有属性 `__proto__ `指向其构造函数的原型对象 `prototype` 。
  并层层向上直到最后一个对象的原型对象为 null。
  null 没有原型并作为原型链中最后一个环节。

  ###### prototype

  function A 有一个叫做 `prototype` 的特殊属性: `{constructor:f}` 这个对象叫原型对象。
  该原型对象可以和`new`操作符一起使用，对原型对象的引用被复制到新实例的内部 [[Prototype]] 属性。
  **箭头函数没有这个属性**。

- #### 垃圾回收

  [[引用计数]]:对象有没有其他对象引用到他。如果没有
  [[标记-清除]]:定期从根对象(root)出发，区分哪些变量是可以获取的，那些事不可以获取的

- #### 事件循环

  ```
  1、执行主执行栈任务，清空执行栈
  2、取出微任务队列中的任务直到清空(如果一直有新的微任务进入队列会一直阻塞宏任务)
  3、取出宏任务队列中的一个任务执行
  4、循环 2、3
  ```

  ```
  function sleep(time) {
    let startTime = new Date();
    while (new Date() - startTime < time) {}
    console.log("<--Next Loop-->");
  }

  setTimeout(() => {
    console.log("timeout1");
    setTimeout(() => {
        console.log("timeout3");
        sleep(1000);
    });
    new Promise(resolve => {
        console.log("timeout1_promise");
        resolve();
    }).then(() => {
        console.log("timeout1_then");
    }).then(() => {
        console.log("666");
    });
    sleep(1000);
  });

  setTimeout(() => {
    console.log("timeout2");
    setTimeout(() => {
        console.log("timeout4");
        sleep(1000);
    });
    new Promise(resolve => {
        console.log("timeout2_promise");
        resolve();
    }).then(() => {
        console.log("timeout2_then");
    });
    sleep(1000);
  });
  ```

  ```
  timeout1
  timeout1_promise
  <--Next Loop-->
  timeout1_then
  666
  timeout2
  timeout2_promise
  <--Next Loop-->
  timeout2_then
  timeout3
  <--Next Loop-->
  timeout4
  <--Next Loop-->
  ```

- #### 事件委托、事件流
  事件流：冒泡、目标处理、捕获。
  合成事件
  兼容不同浏览器的事件流
  不能使用 return false 阻止，需要显示调用 `e.stoppropergation ` 或 `e.preventDefault()`
