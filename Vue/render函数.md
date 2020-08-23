# render

```vue
<template>
  <div>
    <h1>My title</h1>
    Some text content
    <renderCom>
      这里slot部分
    </renderCom>
    <!-- TODO: Add tagline -->
  </div>
</template>

<template>
  <div>
    <h1>My title</h1>
    Some text content
    <renderCom :blogTitle="blogTitle">
      <div>这是slot部分</div>
    </renderCom>
  </div>
</template>

<script>
export default {
  data() {
    return { blogTitle: "blogTitle" };
  },
  components: {
    renderCom: {
      render(h) {
        return h(
          "h1",
          {
            on: {
              click: this.clickHandler
            }
          },
          [this.blogTitle, this.$slots.default]
        );
      },
      methods: {
        clickHandler() {
          alert("clickEvent");
        }
      },
      props: {
        blogTitle: String
      }
    }
  }
};
</script>
```

createElement 到底会返回什么呢？
**其实不是一个实际的 DOM 元素。它更准确的名字可能是 createNodeDescription，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，包括及其子节点的描述信息。我们把这样的节点描述为“虚拟节点 (virtual node)”，也常简写它为“VNode”。“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼。**

```js
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签名、组件选项对象，或者
  // resolve 了上述任何一种的一个 async 函数。必填项。
  "div",

  // {Object}
  // 一个与模板中 attribute 对应的数据对象。可选。
  {
    // 与 `v-bind:class` 的 API 相同，
    // 接受一个字符串、对象或字符串和对象组成的数组
    class: {
      foo: true,
      bar: false
    },
    // 与 `v-bind:style` 的 API 相同，
    // 接受一个字符串、对象，或对象组成的数组
    style: {
      color: "red",
      fontSize: "14px"
    },
    // 普通的 HTML attribute
    attrs: {
      id: "foo"
    },
    // 组件 prop
    props: {
      myProp: "bar"
    },
    // DOM property
    domProps: {
      innerHTML: "baz"
    },
    // 事件监听器在 `on` 内，
    // 但不再支持如 `v-on:keyup.enter` 这样的修饰器。
    // 需要在处理函数中手动检查 keyCode。
    on: {
      click: this.clickHandler
    },
    // 仅用于组件，用于监听原生事件，而不是组件内部使用
    // `vm.$emit` 触发的事件。
    nativeOn: {
      click: this.nativeClickHandler
    },
    // 自定义指令。注意，你无法对 `binding` 中的 `oldValue`
    // 赋值，因为 Vue 已经自动为你进行了同步。
    directives: [
      {
        name: "my-custom-directive",
        value: "2",
        expression: "1 + 1",
        arg: "foo",
        modifiers: {
          bar: true
        }
      }
    ],
    // 作用域插槽的格式为
    // { name: props => VNode | Array<VNode> }
    scopedSlots: {
      default: (props) => createElement("span", props.text)
    },
    // 如果组件是其它组件的子组件，需为插槽指定名称
    slot: "name-of-slot",
    // 其它特殊顶层 property
    key: "myKey",
    ref: "myRef",
    // 如果你在渲染函数中给多个元素都应用了相同的 ref 名，
    // 那么 `$refs.myRef` 会变成一个数组。
    refInFor: true
  },
  [
    // {String | Array}
    // 子级虚拟节点 (VNodes)，由 `createElement()` 构建而成，
    // 也可以使用字符串来生成“文本虚拟节点”。可选。
    ("先写一些文字",
    createElement("h1", "一则头条"),
    createElement(MyComponent, {
      props: {
        someProp: "foobar"
      }
    }))
  ]
);
```

## VNode 必须是唯一的,只能用在一个地方。因此重复渲染时不能套用同一个 vnode，应该用 for 循环

```js
render(h) {
  return h(
    "div",
    new Array(20).fill(20).map(() => {
      return h("p", "hi");
    })
  );
)

```

## v-if、v-for 这些都可以在渲染函数中用 JavaScript 的 if/else 和 map 来重写：

## Babel JSX

## 函数式组件

```js
{
  functional: true,
  // Props 是可选的
  props: {
    // ...
  },
  // 为了弥补缺少的实例
  // 提供第二个参数作为上下文
  render: function (createElement, context) {
    // ...
  }
}
```

**组件需要的一切都是通过 context 参数传递，它是一个包括如下字段的对象：**

props：提供所有 prop 的对象
children：VNode 子节点的数组
slots：一个函数，返回了包含所有插槽的对象
scopedSlots：(2.6.0+) 一个暴露传入的作用域插槽的对象。也以函数形式暴露普通插槽。
data：传递给组件的整个数据对象，作为 createElement 的第二个参数传入组件
parent：对父组件的引用
listeners：(2.3.0+) 一个包含了所有父组件为当前组件注册的事件监听器的对象。这是 data.on 的一个别名。
injections：(2.3.0+) 如果使用了 inject 选项，则该对象包含了应当被注入的 property。

[babel-jsx](https://github.com/vuejs/jsx)

[参考资料](https://cn.vuejs.org/v2/guide/render-function.html)
