---
title: "工厂模式"
date: 2018-10-17 15:04:21
tags: JavaScript
categories: JavaScript
---
#### 定义：工厂模式定义一个用于创建对象的接口，这个接口由子类决定实例化哪一个类。该模式使一个类的实例化延迟到了子类。而子类可以重写接口方法以便创建的时候指定自己的对象类型（抽象工厂）
<!-- more -->
```js
 var CarFactory = function () {}
  CarFactory.BWM = function() {
    this.name = '宝马';
  }
  CarFactory.Audi = function () {
    this.name = '奥迪';
  }
  CarFactory.Benz = function () {
    this.name = '奔驰';
  }

  CarFactory.prototype.drive = function(){
    return '该批次的' + this.name + '添加了自动驾驶功能';
  }

  CarFactory.create = function (carName) {
    if(typeof(CarFactory[carName]) !== 'function'){
      return '暂时不能生产该品牌的车辆';
    }
    if(typeof(CarFactory[carName].prototype.drive !== 'function'){
      CarFactory[carName].prototype = new CarFactory();
    }
    var car = new CarFactory[carName]();
    return car;
  }

  var car = new CarFactory.create('Benz');
  console.log(car.drive());
```