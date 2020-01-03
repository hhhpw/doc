## 元素选择
- querySelector() element
- querySelectorAll() nodeList
- matchesSelector() boolean

## 元素遍历

- childElementCount 返回子元素的个数（不包含文本节点和个数）
- firstElementChild 指向第一个子元素
- lastElementChild 指向最后一个子元素
- nextElementSibling 指向后一个同辈元素
- previousElementSibling 指向前一个同辈元素

# HTML5

## classList

- add 
- remove
- contains boolean
- toggle

## 焦点管理(focus)

- document.activeElement  引用当前DOM中获得焦点的元素
- element.focus() 获取设置焦点
- element.hasFoucs() 元素是否获得焦点，返回布尔值

## HTMLDocument的变化

- loading 正在加载文档
- complete 已经加载完文档

## 自定义数据属性
```js
// <div data-myname='leo'></div>
// 获取
document.getElementByTagName('div')[0].dataset.myname
// 设置
document.getElementByTagName('div')[0].dataset.myname = 'vanyne'
```

## innerHTML、outterHTML

- innnerHTML

读模式：
返回与调用元素的所有子节点（包括元素、注释和文本节点）对应的HTML标记。
写模式：
innerHTML会根据指定的值创建新的DOM树，然后用这个DOM数完全替换元素原先的所有子节点。

innnerHTML下插入script:
 IE8以前可以，现在基本不支持，且在IE8以前需要满足一定条件
 1.script defer属性
 2.script标签必须位于有作用域元素的后面
```js
body.innerHTML = "<input type=\"hidden\"><script defer>alert('hi');<\/script>"
```
另外并不是所有元素都支持innerHTML，例如col、header、table、tbody等。

- outterHTML

**不同之处是在读模式下，返回的是调用它的元素以及所有子节点。
在写模式下，替换调用它的元素以及所有子节点。**


## insertAdjacentHTML/insetAdjacentElement

[insetAdjacentElement](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/insertAdjacentElement)
```js
element.insertAdjacentElement(position, element);
```
参数
- position
A DOMString 表示相对于该元素的位置；必须是以下字符串之一：
  'beforebegin': 在该元素本身的前面.
  'afterbegin':只在该元素当中, 在该元素第一个子孩子前面.
  'beforeend':只在该元素当中, 在该元素最后一个子孩子后面.
  'afterend': 在该元素本身的后面.

- element
  要插入到树中的元素.
- 返回值
  插入的元素，插入失败则返回null.

[insertAdjacentHTML](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/insertAdjacentHTML)

使用方法与insertAdjacentElement相同