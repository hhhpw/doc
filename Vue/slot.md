

#### 定义

  slot,插槽，是组件的一块HTML模板。这块模板显示不显示、以及怎么显示由父组件决定。显示的位置由子组件决定。


#### 分类

  单个插槽（默认插槽、匿名插槽）、具名插槽、作用域插槽（带数据的插槽）


- 默认插槽

```js

// f
<template>
  <div class="father">
    <h3>这里是父组件</h3>
    <son>
      <div>
        SLOT CONTENT
      </div>
    </son>
  </div>
</template>

// s
<template>
  <div>
    <h3>这里是子组件</h3>
    <slot></slot>
    <h3>FOOTER</h3>
  </div>
</template>

```


- 具名插槽

  具有name属性的匿名插槽为具名插槽，目的为了区分不同插槽

  <div slot="a"></div>
  <slot name="a"></slot>


-  作用域插槽


```js
// f

<child>
// slot-scope取得子组件绑定的data数据
// user 可以随便命名
// user {data: ['zhangsan','lisi','wanwu','zhaoliu','tianqi','xiaoba']}，
  <template slot-scope="user">
    <ul>
      <li v-for="item in user.data">{{item}}</li>
    </ul>
  </template>
</child>

// s
<template>
  <div class="child">

    <h3>这里是子组件</h3>
    // 作用域插槽
    <slot  :data="data"></slot>
  </div>
</template>


```