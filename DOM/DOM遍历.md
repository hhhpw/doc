## NodeIterator
(NodeIterator)[https://developer.mozilla.org/zh-CN/docs/Web/API/NodeIterator]

```js
var nodeIterator = document.createNodeIterator(root, whatToShow, filter);
```
属性
这个接口不继承任何属性。

- NodeIterator.root 只读
返回一个Node ，它代表创建 NodeIterator 时指定的根节点。
- NodeIterator.whatToShow 只读
返回一个无符号长整型，它是一个由描述必须呈现的Node类型的常量构成的位掩码。不匹配的节点被跳过，但是如果相关，他们的子节点可能被包括在内。可能的值是：
Constant	Numerical value	Description
NodeFilter.SHOW_ALL	-1 (that is the max value of unsigned long)	显示所有节点。
NodeFilter.SHOW_ATTRIBUTE 	2	显示属性 Attr 节点. 只有当用一个 Attr 节点作为根节点来创建 NodeIterator 时才有意义; 在这种情况下, 这意味着属性节点会出现在迭代或遍历的首位. 因为属性永远不会是其它节点的子节点, 当遍历整个文档树时它们不会出现.
NodeFilter.SHOW_CDATA_SECTION 	8	显示CDATASection 节点。
NodeFilter.SHOW_COMMENT	128	显示Comment 节点。
NodeFilter.SHOW_DOCUMENT	256	显示Document 节点。
NodeFilter.SHOW_DOCUMENT_FRAGMENT	1024	
显示DocumentFragment节点。
NodeFilter.SHOW_DOCUMENT_TYPE	512	显示DocumentType 节点。
NodeFilter.SHOW_ELEMENT	1	显示Element 节点。
NodeFilter.SHOW_ENTITY 	32	显示 Entity 节点. 只有当用一个 Entity 节点作为它的根节点来创建一个 NodeIterator 时才有意义; 在这种情况下,  Entity 节点会出现在迭代或遍历的首位. 因为 Entity  永远不会是其它节点的子节点, 当遍历整个文档树时它们不会出现.
NodeFilter.SHOW_ENTITY_REFERENCE 	16	显示EntityReference 节点。
NodeFilter.SHOW_NOTATION 	2048	显示 Notation 节点. 只有当用一个 Notation 节点作为它的根节点时来创建一个 NodeIterator 才有意义; 在这种情况下,  Notation 节点会出现在迭代或遍历的首位. 因为 Notation  永远不会是其它节点的子节点, 当遍历整个文档树时它们不会出现.
NodeFilter.SHOW_PROCESSING_INSTRUCTION	64	显示ProcessingInstruction 节点。
NodeFilter.SHOW_TEXT	4	显示Text 节点。

- NodeIterator.filter 只读 可以是一个对象也可以是一个函数，遍历的node节点将作为回调参数
返回一个用来选择相关节点的 NodeFilter .
- NodeIterator.expandEntityReferences 只读  
Is a Boolean indicating if, when discarding an EntityReference its whole sub-tree must be discarded at the same time.
- NodeIterator.referenceNode 只读  
返回当前遍历到的 Node .
- NodeIterator.pointerBeforeReferenceNode 只读  
Returns a Boolean flag that indicates whether the NodeIterator is anchored before, the flag being true, or after, the flag being false, the anchor node.

- 方法
这个接口不会继承任何属性。

NodeIterator.detach() 
这个方法不是必需的. 它现在什么也不做. 之前用来告诉引擎，NodeIterator 已经不会再使用，现在已经不做任何事情.
**NodeIterator.previousNode()**
返回前一个 Node ，如果不存在则返回 null.
**NodeIterator.nextNode()**
返回下一个 Node , 如果不存在则返回null .

深度优先遍历console tagname

```js
  let root = document.querySelector("html");
  let filter = (node) => node.tagName.toLowerCase() === 'p' ? NodeFilter.FILTER_ACCEPE : NodeFilter.FILTER_SKIP;
  let iterator = document.createNodeIterator(root, NodeFilter.SHOW_ELEMENT, null, false);
  let node = iterator.nextNode();
  while (node !== null) {
    console.log(node.tagName);
    node = iterator.nextNode();
  }
```

## TreeWakler

```js
  let root = document.querySelector("html");
  let walker = document.createNodeIterator(root, NodeFilter.SHOW_ELEMENT, null, false);

  let node = walker.nextNode();
  while (node !== null) {
    console.log(node.tagName);
    node = walker.nextNode();
  }
  console.log("walker", walker);
```
用法大致和NodeIterator一样，不同点在于除了提供nextNode()和previouNode()，增加:

parentNode(): 遍历到当前节点的父节点
firstChild(): 遍历到当前节点的第一个子节点
lastChild(): 遍历到当前节点的最好一个子节点
nextSibling(): 遍历到当前节点的下一个同辈节点
previousSibling(): 遍历到当前节点的上一个同辈节点