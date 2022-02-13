```html
<body>
  <section class="container">
    <div class="left">
      <div class="menu"></div>
    </div>
    <div class="right">
      <div class="box1"></div>
      <div class="box2"></div>
    </div>
  </section>
</body>
```

### 深度优先遍历(Depth First Search)DFS

遍历完父节点的所有子节点的子节点，再遍历其他兄弟节点。

```js
const DFS = {
  nodes: [],
  do(root) {
    for (let i = 0; i < root.childNodes.length; i++) {
      var node = root.childNodes[i];
      // 过滤 text 节点、script 节点
      if (node.nodeType != 3 && node.nodeName != "SCRIPT") {
        this.nodes.push(node);
        this.do(node);
      }
    }
    return this.nodes;
  },
};

console.log(DFS.do(document.body));
```

### 广度优先遍历(Breadth First Search)BFS

遍历完父节点的所有兄弟节点再遍历其子节点。

```js
// 递归版本
const BFS = {
  nodes: [],
  do(roots) {
    var children = [];
    for (let i = 0; i < roots.length; i++) {
      var root = roots[i];
      // 过滤 text 节点、script 节点
      if (root.nodeType != 3 && root.nodeName != "SCRIPT") {
        if (root.childNodes.length) {
          children.push(...root.childNodes);
        }
        this.nodes.push(root);
      }
    }
    if (children.length) {
      var tmp = this.do(children);
    } else {
      return this.nodes;
    }
    return tmp;
  },
};

// 非递归
const BFS = {
  nodes: [],
  do(roots) {
    var roots = new Array(...roots);
    for (let i = 0; i < roots.length; i++) {
      var root = roots[i];
      // 过滤 text 节点、script 节点
      if (root.nodeType != 3 && root.nodeName != "SCRIPT") {
        if (root.childNodes.length) {
          roots.push(...root.childNodes);
        }
        this.nodes.push(root);
      }
    }
    return this.nodes;
  },
};
console.log(BFS.do(document.body.childNodes));
```
