[参考文档](https://es6.ruanyifeng.com/#docs/class)


```js

class Person {
  // constructor即为构造方法
  // 默认方法，没有也会添加
  constructor(x, y) {
  //ES6 为new命令引入了一个new.target属性，该属性一般用在构造函数之中，返回new命令作用于的那个构造函数
  // 如果子类继承了父类，返回的是子类
    if (new.target !== Person) {
      throw new Error("必须使用new");
    }
    // this代表了实例对象
    this.x = x;
    this.y = y;
    this.name = ["x"];
  }

// 静态方法， 
// static关键字，表示该方法不会被实例继承，但是可以被子类继承
  static test() {
    // this不在指向实例，而是类
    console.log(this)
  }

  // toString将挂载在Person.prototype上
  // 不可枚举，即Object.keys()获取不到，需要Object.getOwnPropertyNames(Person.prototype)
  // ES5方法写，即是可枚举的
  toString() {
    return String(x);
  }
}

// 必须使用new 调用
let p  = new Person();
p.constructor === Person.prototype.constructor; // true

const myClass = class Me {};

// Me只能在Class内部使用，在外部只能使用myClass


```

### 注意点

- 类的内部默认使用严格模式
- 不存在变量提升
- name属性总是返回紧跟在class关键字后面的类名
- 内部方法可是Generator方法
- this指向

```js

class Logger {
  printName(name = 'there') {
    this.print(`Hello ${name}`);
  }

  print(text) {
    console.log(text);
  }
}

const logger = new Logger();
const { printName } = logger;
printName(); // TypeError: Cannot read property 'print' of undefined
```
- static关键字。实例不会继承，但是子类会继承。
- class内部目前只有静态方法，没有静态属性。静态属性提案：#X
- new.target


### 继承 extends


```js

class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}

```

- 子类必须在constructor方法中调用super，否则新建实例会报错

原因在于**子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，
然后在对其进行加工，加上子类的实例属性和方法，如果不调用super方法，子类就得不到this对象**