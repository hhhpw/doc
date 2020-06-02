# URLSearchParams
---
URLSearchParams 接口定义了一些实用的方法来处理 URL 的查询字符串。

``
let searchParams  = new URLSearchParams(window.location.search);
``

- URLSearchParams.append(key, value)
 插入一个指定的键/值对作为新的搜索参数。
- URLSearchParams.delete(key)
 从搜索参数列表里删除指定的搜索参数及其对应的值。无返回。
- URLSearchParams.get()
 获取指定搜索参数的第一个值。
- URLSearchParams.getAll()
 获取指定搜索参数的所有值，返回是一个数组。
- URLSearchParams.has()
 返回 Boolean 判断是否存在此搜索参数。
- URLSearchParams.set()
 设置一个搜索参数的新值，**假如原来有多个值将删除其他所有的值**。
- URLSearchParams.sort()
 按键名排序。
- URLSearchParams.toString()
 返回搜索参数组成的字符串，可直接使用在URL上
 **遍历   for...of**
- URLSearchParams.keys()
返回iterator 此对象包含了键/值对的所有键名。
- URLSearchParams.values()
 返回iterator 此对象包含了键/值对的所有值。[key, value]
- URLSearchParams.entries()
 返回一个iterator可以遍历所有键/值对的对象。