
```js
  let person = {
    name: "Person",
    getName: function (param) {
      console.log("==" + param + "==");
    }
  }
  let student = {
    name: "student",
    getName: function (param) {
      console.log("==student getName==");
    }
  };
```

## Apply
```js
  Function.prototype.myApply = function (context, argsArray) {

    // MyApply方法必然是函数调用的
    if(typeof this !== 'function'){
        throw new TypeError(this + ' is not a function');
    }

    // 2.如果 argArray 是 null 或 undefined, 则argsArray = [];
    if(typeof argsArray === 'undefined' || argsArray === null){
        argsArray = [];
    }
    
    // 3.如果 Type(argArray) 不是 Object, 则抛出一个 TypeError 异常 .
    if(Object.prototype.toString.call(argsArray) !== '[Object Array]'){
        throw new Error('not array');
    }

    // if(typeof thisArg === 'undefined' || thisArg === null){
        // 在外面传入的 thisArg 值会修改并成为 this 值。
        // ES3: thisArg 是 undefined 或 null 时它会被替换成全局对象 浏览器里是window
        // context = widow;
    // }
    // 在这里使用了symbol，是因为如果定义为cxt.func=this;而cxt对象刚好存在func方法，
    // 那就同名覆盖了
    // 也可使用 '__' + new Date().getTime();
    let symbol = Symbol();
    const cxt = context || window;
    cxt[symbol] = this; // 这里的this即为调用的函数 person.getName

    const res = cxt[symbol](...argsArray);
    //  const res = arguments.length > 1 ? cxt[symbol](...argsArray) : cxt[symbol]();
    // 定义在student的[symbol]执行完后，删除该方法，避免影响全局student
    delete cxt[symbol];
    return res;
  }

```

## Call

```js
  Function.prototype.myCall = function (context) {

    if(typeof this !== 'function'){
        throw new TypeError(this + ' is not a function');
    }
    let symbol = Symbol();
    const cxt = context || window;
    cxt[symbol] = this; // 这里的this即为调用的函数 person.getName
    const args = Array.from(arguments).slice(1);
    const res = arguments.length > 1 ? cxt[symbol](...args) : cxt[symbol]();
    delete cxt[symbol];
    return res;
  }

```


## Bind

 bind和call、apply并不相同，
 **bind并没有执行返回结果，而是返回一个函数**，
 结果需要手动执行。

  bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。


```js
  var obj = {
      name: '轩辕Rowboat',
  };
  function original(a, b){
      console.log(this.name);
      console.log([a, b]);
      return false;
  }
  var bound = original.bind(obj, 1); // 传递的参数1,接受
  var boundResult = bound(2); // '轩辕Rowboat', [1, 2] 参数2也接受
  var newBoundResult = new bound(2); 
  console.log(newBoundResult, 'newBoundResult'); 
  // original {name: 2} this指向了new bound生成的对象
```
```js
Function.prototype.myBind = function bind(context) {
  if (typeof this !== 'function') {
    throw new TypeError(this + 'must be a function');
  }
  var self = this; // this是调用myBind的函数
  var args = [].slice.call(arguments, 1);
  var bound = function () {
    console.log("self====", self, "======", this);
    var boundArgs = [].slice.call(arguments);
    if (this instanceof bound) {
      // 通过构造函数 即new关键字, this指向了构造函数实例, 而此时bound已经成为构造函数
      self.apply(this);
    } else {
      self.apply(context, args.concat(boundArgs));
    }
  }

  var Noop = function () { };
  Noop.prototype = this.prototype;
  bound = new Noop();
  // bound.prototype = this.prototype;
  return bound;
}
```


[参考文档](https://segmentfault.com/a/1190000017091983)

