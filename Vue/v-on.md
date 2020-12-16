

$on也是采用了经典的发布订阅者设计模式，首先定义一个事件中心，
通过$on订阅事件，将事件存储在事件中心里面，然后通过$emit触发事件中心里面存储的订阅事件。

```js

Vue.prototype.$on = function (event, fn) {
    const vm: Component = this
    if (Array.isArray(event)) {
        for (let i = 0, l = event.length; i < l; i++) {
            this.$on(event[i], fn)
        }
    } else {
        (vm._events[event] || (vm._events[event] = [])).push(fn)
    }
    return vm

```
