---
title: "策略模式"
date: 2018-10-17 14:58:46
tags: JavaScript
categories: JavaScript
---
#### 定义：定义一系列的算法，把他们一个个封装起来，是他们可以相互替换。
&emsp;&emsp; 例如：根据不同得等级来计算工资
```js
 var calculateBonus = function (performanceLevel, salary) {
    if(performanceLevel === 'S'){
      return salary * 4;
    }
    if(performanceLevel === 'A'){
      return salary * 3;
    }
    if(performanceLevel === 'B'){
      return salary * 2;
    }
  }
  console.log(calculateBonus('B', 2000))  //4000
  console.log(calculateBonus('S', 2000))  //8000
```
&emsp;&emsp; 很明显以上代码非常的冗余，并且如果后期需要维护的话需要在calculateBonus 方法内部进行修改，违反了开放封闭原则。
&emsp;&emsp;使用策略模式重构代码：
&emsp;&emsp;一个基于策略模式的程序至少有两部分组成，第一部分为一组策略类，策略类封装了具体 的算法，并负责具体的计算过程。第二个部分事环境类Context,Context接受客户的请求，随后把请求委托给某一个策略类。
&emsp;&emsp;优化后的代码：
```js
 var strategies = {
  "S": function (salary) {
    return salary * 4;
  },
  "A": function (salary){
    return salary * 3;
  },
  "B": function (salary){
    return salary * 2;
  }
}
var calculateBonus = function (level, salary) {
  return strategies[level](salary);
}
console.log(calculateBonus('B', 2000))  //4000
console.log(calculateBonus('S', 2000))  //8000
```
&emsp;&emsp;策略模式还可以用于比如表单验证等处。