　　1.在vue组件里面，通过dispatch来触发actions提交修改数据的操作。

　　2.然后再通过actions的commit来触发mutations来修改数据。

　　3.mutations接收到commit的请求，就会自动通过Mutate来修改state（数据中心里面的数据状态）里面的数据。

　　4.最后由store触发每一个调用它的组件的更新


　　Vuex的作用：项目数据状态的集中管理，复杂组件(如兄弟组件、远房亲戚组件)的数据通信问题。