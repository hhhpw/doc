# this
  
  通俗来说可以将this认为是代码允许时的执行上下文

  ## this和作用域
  
    this在任何情况下都不指向函数的词法作用域

    this的绑定和声明没有任何关系，只取决于函数的调用方式
    即取决于函数调用环境决定了this

  ## 调用位置

    调用位置即函数的调用位置

    调用栈：为了到达当前执行环境的所调用的所有函数

  ```js

    function A() {
      // 调用栈是A
      // 调用位置是全局
      console.log("A");
      B(); // B的调用位置

    }

    function B() {
      // 调用栈是A->B
      // 调用位置是A中
      console.log("B");
      C(); // C的调用位置
    }

    function C(){
      // 调用栈是A->B->C
      // 调用位置是在B中
      console.log("C");
    }
    
    A(); // A的调用位置
  ```

  ## 绑定规则

   ### 默认绑定

    在非严格模式下，默认绑定在window
    
    在strict mode下，this则绑定在undefined

  ```js
  // 严格模式下调用不影响默认绑定
  // 严格模式下运行影响默认绑定
  var count = 2;

  function A() {
    "use strict"
    console.log(this.a)
  };
  
  A()  // this is undefined

  function B() {
    console.log(this.a)
  }
  
  (function(){
    "use strict"
    B(); // 2
  })()
  ```

  ### 隐式绑定

    常见情况下，在一个对象中包含函数方法。当调用的时候，this隐式绑定在该对象上。

    对象属性引用链只有上一层或者说最后一层在调用位置中起作用。

  ```js

  function foo() {
    console.log(this.a);
  }

  var obj2 = {
    a: 42,
    foo:foo,
  }

  var obj1 = {
    a: 2,
    obj2: obj2,
  }

  obj1.obj2.foo() // 42

  ```
  #### 隐式丢失

      隐式绑定的函数会丢失绑定对象，会应用默认绑定，从而把this绑定在全局对象或者undefined上，取决于是否在严格模式。

  ```js
    function foo() {
      console.log(this.a);
    }

    const obj = {
      a: 10,
      foo,
    }

    var bar = obj.foo;
    
    var a = 99;

    bar(); // 99

    obj.foo() // 10

    // bar是obj.foo的引用，实际上，它引用的是foo函数本身，因此bar()其实是一个不带任何修饰的函数调用，应用了默认绑定。
    
    // 在传入回调函数，将函数当做参数传递其实是一种隐式赋值

  ```    

  ### 显式绑定
        
    call(...,a,b) 立即调用
    apply(..,[arguments]) 立即调用
    bind  返回一个函数

  ### new绑定

    1、创建一个全新的对象
    2、将这个对象的prototype连接到函数
    3、将对象绑定到调用函数的this
    4、如果函数没有返回其他对象，那么会返回这个新对象

  ### 绑定优先级

      1、new 绑定

      2、 显式绑定

      3、 隐式绑定

      4、 默认绑定
  

## 绑定例外

  1、null或undefined作为参数传入call、apply等,这些值在调用时候会被忽略，实际应用的是默认绑定规则。
  针对以上，更安全的应用是利用Object.create(null)

    
    
