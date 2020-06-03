
```html

  <div id="app">
    <input type="text" id="a" v-model='name' />
    {{ name }}
  </div>

```

```js

  function observe(obj, vm) {
    Object.keys(obj).forEach(function (key) {
      defineReactive(obj, key, obj[key]);
    })
  }


  function defineReactive(obj, key, val) {
    var dep = new Dep();
    Object.defineProperty(obj, key, {
      get: function () {
        if (Dep.target) {
          dep.subs.push(Dep.target);
        }
        return val;
      },
      set: function (newVal) {
        if (val === newVal) {
          return
        };
        val = newVal;
        dep.notify();
      }
    })
  }
  //  定义主题对象：添加订阅者和发布者方法
  function Dep() {
    this.subs = [];
  }
  Dep.prototype = {
    // 添加订阅者
    addSub: function (sub) {
      this.subs.push(sub);
    },
    notify: function () {
      // 发布通知
      this.subs.forEach(function (sub) {
        sub.update();
      })
    }
  }

  // 订阅者: 添加数据更新方法
  function Watcher(vm, node, name) {
    Dep.target = this;
    this.name = name;
    this.node = node;
    this.vm = vm;
    this.update(); // 首次渲染
    Dep.target = null;
  }
  Watcher.prototype = {
    update: function () {//更新数据
      this.get();
      this.node.nodeValue = this.value;
    },
    get: function () {//获取数据
      this.value = this.vm.data[this.name];//触发get
    }
  }

  // 指令解析
  function compile(node, vm) {
    var reg = /\{\{(.*)\}\}/;
    if (node.nodeType == 1) {
      // 元素节点 遍历属性 查找相对于的指令
      var attrs = node.attributes;
      for (var i = 0, ln = attrs.length; i < ln; i++) {
        if (attrs[i].nodeName == 'v-model') {
          // v-model 添加实践监听
          var name = attrs[i].nodeValue;
          node.addEventListener("input", function (event) {
            //给相应的data赋值，触发该属性的set方法
            vm.data[name] = event.target.value;
          })
          node.value = vm.data[name];//将data的值赋值给节点
          node.removeAttribute('v-model');
        }
      }
    }
    if (node.nodeType == 3) {
      // 3表示文本节点， 解析模板{{name}}
      if (reg.test(node.nodeValue)) {
        var name = (node.nodeValue).match(reg)[1];
        name = name.trim();
        new Watcher(vm, node, name);//为节点添加订阅者方法
      }
    }
  }


  function nodeToFragment(node, vm) {
    var fragment = document.createDocumentFragment();
    var child;
    while (child = node.firstChild) {
      compile(child, vm);//
      //把el.firstChild即el.children[0]抽出插入到fragment。
      // el.children[0]被抽出，在下次while循环执行firstChild = el.
      // firstChild时读取的是相对本次循环的el.children[1] 以此达到循环转移dom的目的。
      fragment.appendChild(child);
    }
    return fragment;
  }



  function Vue(options) {
    this.data = options.data;
    var data = this.data;

    observe(data, this);

    var id = options.el;
    var dom = nodeToFragment(document.getElementById(id), this);
    document.getElementById(id).appendChild(dom);

  }

  var vm = new Vue({
    el: 'app',
    data: {
      name: 'Lucy'
    }
  });

```
[参考链接](https://blog.csdn.net/quhongqiang/article/details/83784448)