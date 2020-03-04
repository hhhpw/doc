mouseover：当鼠标移入元素或其子元素都会触发事件，所以有一个重复触发，冒泡过程。对应的移除事件是mouseout

mouseenter:当鼠标移除元素本身（不包含元素的子元素）会触发事件，也就是不会冒泡，对应的移除事件是mouseleave

mouseover和mouseenter的异同体现在两个方面：

1.是否支持冒泡
mouseover支持冒泡、mouseenter不支持
2.事件的触发时机