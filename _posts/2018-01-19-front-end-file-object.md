---
layout: post
title: "前端文件对象"
description: 初识Blob、File、FileReader和canvas
date:   2018-01-19 18:27:00 +0800
categories: code
author: bwzou
---
本文主要介绍Blob、File、FileReader和Canvas已经URL等对象属性及作用，最后结合工作遇到的问题介绍他们的应用场景。

### Blob
Blob对象表示不可变得类似文件对象的原始数据。Blob表示不一定是JavaScript原生形式的数据。

**构造函数**：构造函数返回一个新的Blob对象。blob的内容由参数数组中给出的值串联组成。

```js
var blob = new Blob(array,options)
```

* 参数array是一个由ArrayBuffer,ArrayBufferView,Blob,DOMString等对象构成的Array,或者其他类似对象的混合体，它将会被放进Blob。其中DOMString会被编码为UTF-8.

* options 是一个可选的BlobPropertyBag字典，它可能会指定如下两个属性：
	* type，默认值为""，它代表了将会被放入到blob中的数组内容的MIME类型。
	* endings，默认值为"transparent",用于指定包含行结束符\n的字符串如何被写入。它是以下两个值中的一个： "native"，代表行结束符会被更改为适合宿主操作系统文件系统的换行符，或者 "transparent"，代表会保持blob中保存的结束符不变。

**从blob中读取数据**：从Blob中读取内容内容的唯一方法是使用FileReader。以下代码将Blob的内容作为类型数组读取：

```js
var reader = new FileReader();
reader.addEventListener("loadend", function() {
   // reader.result 包含转化为类型数组的blob
});
```



### File
File接口基于Blob，继承了blob的功能并将其扩展使其支持用户系统上的文件。File接口提供有关文件的信息，并允许网页中的 JavaScript 访问其内容。


### FileReader


### Canvas


### URL 

