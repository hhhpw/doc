## 虚拟DOM
Virtual DOM可理解是真是DOM所映射的JS对象，改变任何元素都是在VDOM上先操作的，不直接操作真实DOM。VDOM再和真实DOM比较，DIFF算法，有差距更新真实DOM。

单向数据流：单向数据流是指数据的流向只能由父组件通过props将数据传递给子组件，不能由子组件向父组件传递数据，要想实现数据的双向绑定，只能由子组件接收父组件props传过来的方法去改变父组件的数据，而不是直接将子组件的数据传递给父组件。



1、react是单向数据流，react在prps或者state发生改变后，会重新走渲染的流程。如果shouldComponetUpdate返回的是true就继续渲染，反正则不渲染。vue也是基于数据的，通过Object的defineProperty去监听，实现双向绑定。
7
2、react使用JSX语法去生成JS、HTML、CSS等。vue这通过vue文件将HMTL模板、CSS、JS结合在一起。

3、react语法上更为灵活，vue则提供了多种API，较为模板化。

4、vue-cli，create react app。vuex redux。router等

## 相同点

1. 都在使用虚拟DOM
2. 提供响应式和组件化的视图组件
3. 都有丰富的周边生态(vue-router、react-router、vuex、redux)等

## 不同点

1. 在react中，一切都是js编写的,all in js,使用jsx语法。vue把html、css、js组合到一起，到写到对应的模板里
2. 构建工具。creat-react-app、vue-cli
3. react单向数据流。vue双向绑定
4. 周围生态。redux、vuex、router的等