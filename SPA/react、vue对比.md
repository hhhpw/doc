### MVVM
MVVM: model-view-viewmodel
Model:数据模型
View:UI组件
ViewModel:监听数据模型的改变和控制视图的行为、处理交互行为，可以理解为同步View和Model的对象，连接两者.
在MVVM架构下，View 和 Model 之间并没有直接的联系，而是通过ViewModel进行交互，Model 和 ViewModel 之间的交互是双向的， 
因此View 数据的变化会同步到Model中，而Model 数据的变化也会立即反应到View 上。
ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而View 和 Model 之间的同步工作完全是自动的，无需人为干涉。


### MVC


---

1、react是单向数据流，react在prps或者state发生改变后，会重新走渲染的流程。如果shouldComponetUpdate返回的是true就继续渲染，反正则不渲染。vue也是基于数据的，通过Object的defineProperty去监听，实现双向绑定。

2、react使用JSX语法去生成JS、HTML、CSS等。vue这通过vue文件将HMTL模板、CSS、JS结合在一起。

3、react语法上更为灵活，vue则提供了多种API，较为模板化。

4、vue-cli，create react app。vuex redux。router等

虚拟DOM
Virtual DOM可理解是真是DOM所映射的JS对象，改变任何元素都是在VDOM上先操作的，不直接操作真实DOM。VDOM再和真实DOM比较，DIFF算法，有差距更新真实DOM。

单向数据流：单向数据流是指数据的流向只能由父组件通过props将数据传递给子组件，不能由子组件向父组件传递数据，要想实现数据的双向绑定，只能由子组件接收父组件props传过来的方法去改变父组件的数据，而不是直接将子组件的数据传递给父组件。


