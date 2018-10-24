---
title: python-test
date: 2018-10-24 09:33:33
tags: [Python,test]
categories: Python
---

> 此文摘自[阮一峰JS教程](https://wangdoc.com/javascript/)

#### DOM
- 什么是DOM
- DOM与html， DOM与JavaScript的关系
- DOM的结构是怎么样的
- JS是如何操控DOM的

##### Node接口
**节点Node**
- 节点是DOM的最小组成单位
- nodeType
- nodeValue
    - nodeValue属性返回一个字符串，表示当前节点本身的文本值，该属性可读写。
    - 只有文本节点（text）、注释节点（comment）和属性节点（attr）有文本值，因此这三类节点的nodeValue可以返回结果，其他类型的节点一律返回null。同样的，也只有这三类节点可以设置nodeValue属性的值，其他类型的节点设置无效
- parentNode
    - 对于一个节点来说，它的父节点只可能是三种类型：元素节点（element）、文档节点（document）和文档片段节点（documentfragment）

- replaceChild(newNode, oldNode)  替换节点
- normalize： 清除空的文本节点并且合并相邻的文本节点


##### NodeList && Htmlcollection 接口
**NodeList**
- NodeList实例是一个类似数组的对象，它的成员是节点对象;通过以下方法可以得到NodeList实例。
    - Node.childNodes
    - document.querySelectorAll()等节点搜索方法
- NodeList实例很像数组，可以使用length属性和forEach方法。但是，它不是数组，不能使用pop或push之类数组特有的方法。
- NodeList.prototype.keys(): 返回键名的遍历器
- NodeList.prototype.values(): 返回键值的遍历器
- NodeList.prototype.entries(): 返回同时包含键值和键名的遍历器
- 可以使用`for...of`来遍历遍历器对象

> 注意，NodeList 实例可能是动态集合，也可能是静态集合。所谓动态集合就是一个活的集合，DOM 删除或新增一个相关节点，都会立刻反映在 NodeList 实例。目前，只有Node.childNodes返回的是一个动态集合，其他的 NodeList 都是静态集合

```js
document.body.childNodes instanceof NodeList // true


var children = document.body.childNodes;
for (var i = 0; i < children.length; i++) {
  var item = children[i];
}


var children = document.body.childNodes;
children.length // 18
document.body.appendChild(document.createElement('p'));
children.length // 19


var children = document.body.childNodes;
for (var key of children.keys()) {
  console.log(key);
}
for (var entry of children.entries()) {
  console.log(entry);
}
```

**HTMLcollection**
- HTMLCollection是一个节点对象的集合，只能包含元素节点（element），不能包含其他类型的节点。它的返回值是一个类似数组的对象，但是与NodeList接口不同，HTMLCollection没有forEach方法，只能使用for循环遍历
- 返回HTMLCollection实例的，主要是一些Document对象的集合属性，比如document.links、docuement.forms、document.images等
- HTMLCollection实例都是动态集合，节点的变化会实时反映在集合中
- 如果元素节点有id或name属性，那么HTMLCollection实例上面，可以使用id属性或name属性引用该节点元素。如果没有对应的节点，则返回null
- HTMLCollection.prototype.length
- HTMLCollection.prototype.namedItem():namedItem方法的参数是一个字符串，表示id属性或name属性的值，返回对应的元素节点,如果没有对应的节点，则返回null.

```js
document.links instanceof HTMLCollection // true

// HTML 代码如下
// <img id="pic" src="http://example.com/foo.jpg">
var pic = document.getElementById('pic');
document.images.pic === pic // true
document.images.namedItem('pic') === pic // true
```
