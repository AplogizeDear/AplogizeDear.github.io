---
title: javascript
date: 2020-11-07 16:27:08
tags: js基础
categories: 知识
---

###  总结来自红宝书《javascript高级程序设计》

  光说不练假把式！

1. #### javascript语言简介

   javascript是一种解释型的脚本语言，目的是能够在客户端的网页中增加**动态效果**和**交互能力**

   总的来说分为三部分

   ECMAscript 核心 -  主要处理一些语句语法类型关键字之类的东西

   BOM 浏览器对象模型  暴露出一些API来操作浏览器 比如说滑动，弹框，视口大小

   DOM  文档对象模型   暴露出来API用来处理dom元素

2. #### script 标签

   属性：1. src   2. type  默认text/javascript   3.async 立即执行脚本 但是不妨碍界面中的其他操作   defer 文档全部解析和显示完执行

   多个外链js脚本在没有async和defer的情况下会照常执行。

3. #### 基本概念

   ##### 3.1 语法

   ​	3.1.1 区分大小写

   ​	3.1.2 标识符 变量、函数、属性的名字  多采用驼峰命名（firstName）

   ​	3.1.3 严格模式

   ```javascript
     function dosomething(){
       "use strict"
     }
   ```

   ##### 3.3 变量

   1. ECMAscript的变量是一种松散型的，也就是说他的变量可以用来保存任何类型的数据。每个变量仅仅表示的是一个占位符。

      ```javascript
      var message;
      ```

      Var 表示的是操作符，像这样没有赋值的占位符都会存一个值为undefined

   2. 数据类型

      五种简单数据基本类型:null String undefined boolean Number 

      一种复杂基本数据类型：object 或则Null

      因为变量比较松散，所以需要通过一个标识符来确定类型：typeof

   3. null   

      空对象指针 typeof null = object

      如果未来打算保存对象，那么这个值可以初始化为null

   4. String

      字符串特点：一经创建，他的值就不能在改变，如果想要改变某个变量保存的字符串，首先要销毁原来的字符串。然后用新的字符串来填充该变量

      toString(); 接受一个参数，指定进制，可输出进制格式表示的任意进制值

      ```javascript
      var age = 11;
      var ageString = age.toString() //"11"
      var num = 10;
      num.toString(2); //"1010"
      var found = true;
      var foundString = found.toString() // "true"
      ```

      

   5. undefined  未经初始化的值默认会取得undefined,这个值重来就不会用来进行显式的声明

   6. Number 

      isFinite() 函数，用来确认一个数值是否是无穷的

      isNaN() 判断一个值是否是NaN

      Number()  把各种数据类型转换为数值

      ```javascript
      var num1 = Number("hello");  //NaN
      var num2 = Number("");  //0
      var num3 = Number("000111"); //111
      var num4 = Number(true); //1
      ```

      parseInt() 第二个参数可以指定基数

      ```javascript
      var num1 = parseInt("123blue");  // 123
      var num2 = parseInt("");  //NaN
      var num3 = parseInt("0XA"); //10
      var num4 = parseInt(22.5); // 22
      ```

      parseFloat()

      ```javascript
      var num1 = parseInt(22.5); //22.5
      ```

   7. boolean

   8. object

      对象，一组数据和功能的集合

      object的每个实例都具有下列属性和方法

      1. constructor 保存用于创建对象的函数
      2. o.hasOwnProperty("name"); 用于检索对象中是和否有该属性。

   ##### 3.4 操作符

   ​	一元操作符  

   ​		递增递减操作符(++、--) 

   ​		一元加减操作符(主要用于转换正负)

   ​	布尔操作符 ！&&  ||

   ​	乘性操作符 * /  %

   ​	加性操作符 + -

   ​	关系操作符 >    <    >=    <=    

   ​	相等操作符  ==     !=    ===	  !==

   ​	条件操作符	var max = num1 > num2 ? num1:num2;

   ##### 3.5 语句 （流控制语句）

   ​	if语句

   ​	do-while 语句

   ​	while 语句

   ​	for 语句。

   ​	for-in语句 用来遍历对象中的所有属性

   ​	switch 语句

   ​	label语句（没有使用过）

   ##### 3.6 函数

   1. 参数

      ECMA中的参数是用数组表示的。命名的参数知识提供便利，并不是必须的。

      可以通过arguments来访问函数传递进来的参数。注意一点，这种情况，通过严格模式是没有用的。

      函数重复命名之后，只会执行最后一个。（不会重载）

   ##### 3.7 变量作用于和内存问题

   ​	javascript的变量是一个松散的值，他不会保存变量的数据类型，因此，在生命周期当中可能在某个时刻发生变化。Ts

   的出现正是以为这个原因。

   1. 基本数据类型和引用基本类型

      基本：undefined null number string boolean。这五种数据类型是按值访问，可以操作保存在变量中的实际的值。

      引用：引用类型的值是保存在内存中的对象。Javascript不能直接操作对象的内存空间。是操作对象的时候，实际上是在控制对象的应用。

      1. 动态的属性

         基本：不可添加属性

         引用：可以添加属性和方法

      2. 复制变量值

         基本：复制后会在内存空间重新定义一个值，俩者完全独立

         引用：复制后的值的副本实际上是一个指针，这个值指针指向堆内存中的一个对象。俩个变量使用的是一个对象。

      3. 传递参数

         ECMAscript中的值都是通过值去传递的

         基本：把函数外部的值赋值给函数内部，也就是所谓的arguments

         引用：会把这个值在内存中的地址复制给一个局部变量，去最原始的引用不会发生变化

      4. 检测类型

         typeof检测基本数据类型 string number boolean undefined，除了null和object

         instaneof 检测引用的基本类型

         ```javascript
         personal instanceof Object;
         arr instanceof Array;
         reg instanceof RegExp;
         ```

      5. 执行环境及其作用域

        1. 执行环境：执行环境定义了变量和函数有权访问的其他数据，决定了他们各自的行为。
        2. 变量对象：环境中定义的变量和函数都保存在这个对象中。
        3. 全局执行环境：window对象，所有的全局变量和函数都是作为window对象的属性和方法创建的
        4. 每个函数都有自己的执行环境，当一个函数执行的时候，这个函数的执行环境会被压入一个环境栈中，当函数执行完毕之后。栈将其环境弹出，把控制权返回给外层的环境
        5. 作用域链：保证对执行环境有权访问的所有变量和函数的有序访问

    

         

   

   ​	

   















 
