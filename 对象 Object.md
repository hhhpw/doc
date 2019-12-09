
  ### 意义

        js数据类型的一种，是引用类型。内置对象包括String、
        Number、Boolean、Object、Function、Array、Date、
        RegExp、Error等

  ### 属性描述符 getOwnerPropertyDescriptor

    ```js
    const obj = {
      a: 'leo',
    };
    // 使用
    Object.getOwnPropertyDescriptor(obj, "a")
    // 输出
    { 
      value: "leo",  // 值
      writable: true, // 可写
      enumerable: true,  // 可枚举 （for in 遍历）
      // 可配置，单向操作，设置为false后不能再去修改描述符属性
      // 当configurable为false时,writable为true仍可修改为true
      configurable: true  
    }
    ```


  ### 复制

  - 概念

        值类型和引用类型。深复制递归去copy，直到发现不是引用用类型。

  - 深复制

        对象里的元素类型多种多样的，没有内置的copy方法去实现
    
      ```js
        // 对于部分适用情况，对象值不为undefined，没有函数情况，可使用
        const cloneDeep = 
          (obj) => JSON.parse(JSON.stringify(obj));

        // 适用大部分情况
        const cloneDeep = function cloneDeep(obj) {
          if (obj === null) return null;
          let newObj = obj instanceof Array ? [] : {};
          for (let i in obj) {
          if (obj.hasOwnProperty(i)) {
          // 过滤继承属性
            newObj[i] = typeof obj[i] === 'object' ? 
            cloneDeep(obj[i]) : obj[i];
          }
          }
          return newObj;
        };

      ```

  - 浅复制

      ```js
      Object.assign({}, someObj);
      ```


  ### 常用属性的方法

   - in
   
          返回布尔值。会检查该属性是否在对象自身以及原型链上

  - for in

        任意顺序遍历一个对象自有的、可枚举的、继承的、非symbol的属性

  - Object.create(proto,[propertiesObject])
    Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。 
  ```
  proto:新创建对象的原型对象
  propertiesObject:可选。要添加到新对象的可枚举（新添加的属性是其自身的属性，而不是其原型链上的属性）的属性。
  ```
  
  - Object.assgin()

  - Object.keys(obj)

  - Object.values(obj)

  - Object.entries(obj)

  - Object.getOwnPropertyNames(obj)

        Object.getOwnPropertyNames() 返回一个数组。
        元素都是自有的、可枚举、不可枚举的属性字符串。


  - Object.hasOwnProperty(prop)

        hasOwnProperty() 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是是否有指定的键）

  - Object.getOwnPropertyDescriptor(object, prop)

        返回属性描述符对象，包括value,writable,enumerable,configurable
        四个属性

  - Object.is(val1, val2)

        表示两个参数是否相同的布尔值

  - Object.defineProperty(obj, prop, descriptor)
  Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

    ```js
    对象里目前存在的属性描述符有两种主要形式：数据描述符和存取描述符。
    数据描述符是一个具有值的属性，该值可能是可写的，也可能不是可写的。
    存取描述符是由getter-setter函数对描述的属性。
    描述符必须是这两种形式之一；不能同时是两者。
      let obj = { a: 10 };
      Object.defineProperty(obj, "b", {
        value: 10,
        writable: true,
        configurable: true,
        enumerable: true,
        });
      Object.defineProperty(obj, "b", {
        get() {

        },
        set() {
          
        }
        });
      console.log(obj.b);
    ```
    

        
  - Object.preventExtensions(obj)

        禁止给对象添加属性

  - Object.isExtensible(obj)

  - Object.seal(obj)

        封闭一个对象，阻止添加新属性并将所有现有属性标记为不可配置。
        当前属性的值只要可写就可以改变(当前不可删除)

  - Object.isSealed(obj)


  - Object.freeze(obj)

        最高级别的不可变性
    
  - Object.isFrozen(obj)