---
title: "代理模式"
date: 2018-10-17 14:53:32
tags: JavaScript
categories: JavaScript
---
#### 定义：为一个对象提供一个代用品或者占位符，以便控制对他的访问
&emsp;&emsp;代理模式的关键在于，当客户不方便直接访问一个对象或者不满足需要的时候，提供一个替身对象来控制对这个对象的访问，客户实际上访问的时替身对象。替身对象对请求做一定的处理之后，再把请求转交给本体对象。
&emsp;&emsp;示例：小明追求女神，送花的故事
&emsp;&emsp;小明可以直接把花送给女神
<!-- more -->
```js
    // 代理模式
    var Flower = function(name){
      this.name = name;
    }

    var xiaoming = {
      sendFlower: function (target){
        var flower = new Flower('向日葵');
        target.reciveFlower(flower);
      }
    }
    
    var xiaoli = {
      reciveFlower: function (flower) {
        console.log('xiaoli收到了' + flower.name);
      }
    }
    xiaoming.sendFlower(xiaoli);
```
  &emsp;&emsp;但是直接把花给女神，女神可能心情好，也可能心情不好，所以只有50%的概率接受花，那么这时候就需要将花给女神闺蜜，当女神心情好的时候将花送给女神。
 &emsp;&emsp;就在这个时候，老王上线了👻，小明先将花给老王，老王监听小丽心情，当小丽心情好的时候再将花转交给小丽（**控制不同权限的对象对目标对象的访问**）。当然花也挺贵的，万一买了花女神正好心情不好那就要吃土了，所以把钱给老王，等小丽心情好的时候再去买。（**将开销很大的对象延迟到真正需要它的时候再去创建它**）
 &emsp;&emsp;修改代码如下：
 ```js
    var xiaoming = {
      sendFlower: function (target){
        target.reciveFlower();
      }
    }
  
    var xiaoli = {
      reciveFlower: function (flower) {
        console.log('xiaoli收到了' + flower.name);
      },
      listenGoodMood: function (fn){
        setTimeout(function(){
          fn();
        }, 5000)
      }
    }

    var laowang = {
      reciveFlower: function (){
        xiaoli.listenGoodMood(function(){
          var flower = new Flower('玫瑰花🌹');
          xiaoli.reciveFlower(flower);
        })
      }
    }
    xiaoming.sendFlower(laowang);
 ```
&emsp;&emsp; 利用代理模式可以实现图片预加载，当图片资源未加载出时，为了避免长时间的白屏时间，是u用加载图进行占位。代码如下：
```js
var myImage = (function(){
  var imgNode = document.createElement('img');
  $("#div").append(imgNode);
  return {
    setSrc: function (src){
      imgNode.src = src;
    }
  }
})();

var proxyImage = (function(){
  var img = new Image;
  img.onload = function (){
    myImage.setSrc(this.src);
  }
  return {
    setSrc: function (src){
      myImage.setSrc('http://img.zcool.cn/community/01191d57b4265f0000018c1b95706d.gif');	// 这应该为本地loading动图
      setTimeout(function(){  // 模拟网络请求
        img.src = src;
      }, 3000)
    }
  }
})();
proxyImage.setSrc('http://img.pconline.com.cn/images/upload/upc/tx/itbbs/1309/25/c29/26303615_1380079893379_500x500.jpg');
```
&emsp;&emsp; 当然图片预加载不使用代理模式也可以实现，代码如下：
```js
var loadImage = (function (){
  var imgNode = document.createElement('img');
  $("#div").append(imgNode);
  var img = new Image;
  img.onload = function (){
    imgNode.src = img.src;
  }
  return {
    setSrc: function (src){
      imgNode.src = 'http://img.zcool.cn/community/01191d57b4265f0000018c1b95706d.gif';
      setTimeout(function(){ // 模拟网络请求
        img.src = src;
      },3000);
      
    }
  }
})();
loadImage.setSrc('http://img.pconline.com.cn/images/upload/upc/tx/itbbs/1309/25/c29/26303615_1380079893379_500x500.jpg');
```
&emsp;&emsp; 但是可以看出这种很明显违反了单一职责原则，违反了该原则，当一个对象承担的职责过多时，那么这个对象将会变的非常庞大，并且维护起来将会非常困难。面向对象设计鼓励将行为细化，即为“高内聚、低耦合”的体现。其实当我们违反了单一职责原则的同时，也会违反开放-封闭原则。