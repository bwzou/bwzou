---
layout: post
title: "前端文件对象"
description: 初识Blob、File、FileReader和Canvas
date:   2018-01-19 18:27:00 +0800
categories: frontend
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
FileReader 对象允许Web应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 File 或 Blob 对象指定要读取的文件或数据。

**方法**

  方法定义	                     | 	 描述 					
---------------------------------|-------------------------------
 abort():void 	                 | 终止文件读取操作 	 
 readAsArrayBuffer(file):void    | 异步按字节读取文件内容，结果用ArrayBuffer对象表示
 readAsBinaryString(file):void   | 异步按字节读取文件内容，结果为文件的二进制串
 readAsDataURL(file):void        | 异步读取文件内容，结果用data:url的字符串形式表示
 readAsText(file,encoding):void  | 异步按字符读取文件内容，结果用字符串形式表示

**事件**

  方法定义	                     | 	 描述 					
---------------------------------|-------------------------------
 onabort 	                     | 终止文件读取操作 	 
 onerror                         | 异步按字节读取文件内容，结果用ArrayBuffer对象表示
 onload                          | 异步按字节读取文件内容，结果为文件的二进制串
 onloadend                       | 异步读取文件内容，结果用data:url的字符串形式表示
 onloadstart                     | 异步按字符读取文件内容，结果用字符串形式表示
 onprogress                      | 在读取数据过程中周期性调用

### Canvas
canvas中有toDataURL函数，可以将canvas转为dataURL形式的base64编码。


### URL 
调用 URL 对象的 createObjectURL 方法，传入一个 File 对象或者 Blob 对象，能生成一个链接。
```js
  var objecturl =  window.URL.createObjectURL(blob);
```
上面的代码会对二进制数据生成一个 URL，这个 URL 可以放置于任何通常可以放置 URL 的地方，比如 img 标签的 src 属性。需要注意的是，即使是同样的二进制数据，每调用一次 URL.createObjectURL 方法，就会得到一个不一样的 URL。

### 项目中遇到的例子
在项目需要实现一个功能，就是表单内容的缓存，而表单内容涉及到File文件，其实就是图片封面。当用户跳转之后，原先选择的内容会有所保存，再跳转回来之后就会自动填充。缓存采用的是localStorage，但是localStorage只能存储字符串，不能直接存储文件对象，所以需要将图片文件转化成base64编码的字符串，想到两种方法：

**使用Canvas**
```js
  function toBase64(file){
    let storageFiles={},
    elephant = document.getElementById("dialogImage"),
    date = new Date(),
    todayDate = (date.getMonth()+1).toString()+date.getDate().toString();
    localStorage.removeItem("storageFiles");
    
    elephant.addEventListener("load", function () {
      let imgCanvas = document.createElement("canvas");
      let imgContext = imgCanvas.getContext("2d");
      imgCanvas.width = elephant.width;
      imgCanvas.height = elephant.height;
      imgContext.drawImage(elephant, 0, 0, elephant.width, elephant.height);
      storageFiles.elephant = imgCanvas.toDataURL("image/png");
      storageFiles.date = todayDate;
	  try {
         localStorage.setItem("storageFiles", JSON.stringify(storageFiles));
      } catch (e) {
         console.log("Storage failed:" + e);
      }
	})
}
```

**使用FileReader**
```js
  function toBase64(file){
    let storageFiles = {};
    storageFiles.date = new Date();
    storageFiles.name = file.name;

    let reader = new FileReader();
    reader.onload = function() {
      storageFiles.elephant = reader.result;
      try {
        localStorage.setItem("storageFiles", JSON.stringify(storageFiles));
      } catch (e) {
        console.log("Storage failed:" + e);
      }
    };
    reader.readAsDataURL(file);
  }
```

将base64编码的字符串转化成文件：
```js
  function toFile(){
      let storageFiles = JSON.parse(localStorage.getItem("storageFiles")) || {};
      if(storageFiles.elephant){
         let arr = storageFiles.elephant.split(','),mime = arr[0].match(/:(.*?);/)[1],
         bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
         while(n--){
           u8arr[n] = bstr.charCodeAt(n);
         }
         let theFile = new Blob([u8arr], {type:mime});
         let fileArr = [];
         fileArr.push(theFile);
         thiz.ruleForm.file = new File(fileArr, storageFiles.name, {type: mime});
      }
    }  
```

PS：canvas转换法转换之后是两张不一样的图片。