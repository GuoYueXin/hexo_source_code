---
title: 单例模式
tags: JavaScript
date: 2018-10-17 00:00:00
categories: JavaScript
---

定义：保证一个类仅有一个实例，并提供一个访问它的全局访问点
<!-- more -->
实例代码：
```js
  // 精简版单例模式
    var getSingle = function (fn) {
      var result;
      return function () {
        return result || ( result = fn.apply(this, arguments) );
      }
    }
```
&emsp;&emsp;单例模式应用场景：比如当一个页面需要一个登陆框，无论在任何状态都只有一个登陆框，那我们可以利用单例模式来控制有且只有一个登陆框。
&emsp;&emsp;如下：当我们渲染完一个列表之后，给这个列表绑定click事件，如果是通过Ajax动态的向列表里面追加数据，在使用事件代理的情况下，click事件实际上只需要在第一次渲染列表的时候被绑定一次，但是我们不想去判断是否是第一次渲染列表，当然我们可以使用jQuery给dom元素绑定one事件，但是我们也可以使用单例模式来解决。代码如下：
```js
  var bindEvent = function() {
    document.getElementById('div').onclick = function(){
      alert('click');
    }
    return true;
  }
  var divClick = getSingle(bindEvent);
  divClick();
  divClick();
  divClick();
```
&emsp;&emsp;可以看到在后面我们执行了3次该方法，但实际上只执行了一次。至于单例模式的其他用途可以自己发挥。
&emsp;&emsp;注：要根据业务复杂程度来判断是否有必要使用该模式，其他设计模式反之亦然，当业务逻辑不是很复杂时，使用设计模式反而会使代码很冗余。