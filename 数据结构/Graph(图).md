### 概念

图表示为 G,V 为一组顶点，E 为一组边。
则 G=(V, E)

相邻顶点：
度：
路径：

```js
// 创建图
class Graph {
  constructor() {
    this.vertices = [];
    this.adjList = new Map();
  }

  addVertex(v) {
    if (!this.vertices.includes(v)) {
      this.vertices.push(v);
      this.adjList.set(v, []);
    }
  }

  addEdge(v, w) {
    if (!this.adjList.get(v)) {
      this.addVertex(v);
    }
    if (!this.adjList.get(w)) {
      this.addVertex(w);
    }
    // 这样做是为了保证两个点是双向建立边的
    // 否则只存在v => w
    // 不存在w => v
    let t = this.adjList.get(v);
    t.push(w);
    this.adjList.set(v, t);
    let o = this.adjList.get(w);
    o.push(v);
    this.adjList.set(w, o);
  }

  getVertices() {
    return this.vertices;
  }

  getAdjList() {
    return this.adjList;
  }

  toString() {
    let s = "";
    for (let i = 0; i < this.vertices.length; i++) {
      s += `${this.vertices[i]} ->`;
      const values = this.adjList.get(this.vertices[i]);
      for (let j = 0; j < values.length; j++) {
        s += `${values[j]}`;
      }
      s += "\n";
    }
    return s;
  }
}
//// demo
const g = new Graph();
const arr = ["A", "B", "C", "D", "E"];
for (let i = 0; i < arr.length; i++) {
  g.addVertex(arr[i]);
}
const edges = [
  ["A", "B"],
  ["A", "C"],
  ["C", "D"],
  ["C", "E"],
];

for (let i = 0; i < edges.length; i++) {
  g.addEdge(edges[i][0], edges[i][1]);
}

console.log(g.toString());
```
