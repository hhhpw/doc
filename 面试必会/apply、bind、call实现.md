```js
let person = {
  name: "Person",
  color: "red"
};
let student = {
  name: "student",
  getName: function (param) {
    console.log("student", this.color);
    return "done";
  }
};
```

## Apply

```js
Function.prototype.myApply = function (context, argsArray) {
  // MyApply方法必然是函数调用的
  if (typeof this !== "function") {
    throw new TypeError(this + " is not a function");
  }

  // 2.如果 argArray 是 null 或 undefined, 则argsArray = [];
  if (typeof argsArray === "undefined" || argsArray === null) {
    argsArray = [];
  }

  // 3.如果 Type(argArray) 不是 Object, 则抛出一个 TypeError 异常 .
  if (Object.prototype.toString.call(argsArray) !== "[object Array]") {
    throw new Error("not array");
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
  cxt[symbol] = this; // 这里的this即为调用的函数 student.getName

  const res = cxt[symbol](...argsArray);
  //  const res = arguments.length > 1 ? cxt[symbol](...argsArray) : cxt[symbol]();
  // 定义在student的[symbol]执行完后，删除该方法，避免影响全局student
  delete cxt[symbol];
  return res;
};

// 调用
let res = student.getName.myApply(person);
```

## Call

```js
Function.prototype.myCall = function (context) {
  if (typeof this !== "function") {
    throw new TypeError(this + " is not a function");
  }
  let symbol = Symbol();
  const cxt = context || window;
  cxt[symbol] = this; // 这里的this即为调用的函数 student.getName
  const args = Array.from(arguments).slice(1);
  const res = arguments.length > 1 ? cxt[symbol](...args) : cxt[symbol]();
  delete cxt[symbol];
  return res;
};
```

## Bind

bind 和 call、apply 并不相同，
**bind 并没有执行返回结果，而是返回一个函数**，
结果需要手动执行。

bind() 函数会创建一个新函数（称为绑定函数），新函数与被调函数（绑定函数的目标函数）具有相同的函数体(不仅仅函数体，原型链也要复制)。
当新函数被调用时 this 值绑定到 bind()的第一个参数，该参数不能被重写。
绑定函数被调用时，bind()也接受预设的参数提供给原函数。
一个绑定函数也能使用 new 操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给新函数。

简单来说：
1、函数 foo.getValue 调用 bind 的时候，会返回一个新的函数 newGetValue。
2、newGetValue 和 foo.getValue 函数体是一样的，原型链也要继承。
3、newGetValue 函数被调用时的 this 是指向对象 foo（传给 bind 的第一个参数）。
4、bind 函数被调用时传递的参数，会在 newGetValue 被调用时传递，并且排在实参的最前面。
5、new newGetValue() 时，会把 newGetValue 当成一个构造函数，this 自动被忽略，参数依旧可以传。

```js
var foo = {
  value: "233",
  getValue: function () {
    console.log(this.value);
    console.log(arguments[0], arguments[1]);
  }
};

var newGetValue = foo.getValue.bind(foo, 1);
newGetValue(2); // 233 1 2
```

```js
var obj = {
  name: "轩辕Rowboat"
};
function original(a, b) {
  console.log(this.name);
  console.log([a, b]);
  return false;
}
var bound = original.bind(obj, 1); // 传递的参数1,接受
var boundResult = bound(2); // '轩辕Rowboat', [1, 2] 参数2也接受
var newBoundResult = new bound(2);
console.log(newBoundResult, "newBoundResult");
// original {name: 2} this指向了new bound生成的对象
```

```js
Function.prototype.myBind = function bind(context) {
  if (typeof this !== "function") {
    throw new TypeError(this + "must be a function");
  }
  var that = this; // this即为调用myBind的函数
  var args = [].slice.call(arguments, 1);
  var bound = function () {
    var boundArgs = [].slice.call(arguments);
    if (this instanceof bound) {
      // 通过构造函数 即new关键字, this指向了构造函数实例, 而此时bound已经成为构造函数
      that.apply(this, args.concat(boundArgs));
    } else {
      // 普通函数,this 指向绑定的context
      that.apply(context, args.concat(boundArgs));
    }
  };

  // 修改返回函数的 prototype 为绑定函数的 prototype，实例就可以继承函数的原型中的值
  // bound.prototype = this.prototype;
  // 不直接绑定的原因在于 修改bound.prototype 也会修改函数的prototype,
  // 因此使用空函数来中转
  var Noop = function () {};
  Noop.prototype = this.prototype;
  bound.prototype = new Noop();
  // 以上可以使用Object.create(this.prototype)
  return bound;
};
```

---

### 注意
使用null、undefined作为this的绑定对象传入call、apply、bind，这些值在调用时候会被忽略，实际应用的是默认规定。

[参考文档](https://segmentfault.com/a/1190000017091983)
[参考文档](https://blog.csdn.net/smallsun_229/article/details/80298147)
