## 开篇

---
当谈及javascript的继承时，往往都离不开原型和原型链。那么继承、原型以及原型链之间的关系是怎样的呢？而事实上，网络上已经有很多前辈帮我们讲解它们之间的关系，往往我们也能看到下面的这张关系图谱：

![继承、原型、实例关系图](./imgs/prototype.jpg)

事实上，这些关系已经被详尽说明了，本文主要还是以它们之间的关系来发掘JavaScript的有趣之处。当然首先咱们还是得清楚这三者究竟是什么又代表着什么，有劳各位听在下道来了。

---
- ***继承***

> 继承是面向对象的一种概念，也是面向对象的基本特征。具备的功能主要是：**子类继承父类后，子类具有父类的属性和方法**。实际上我们首先要知道，js中并没有提供类的实现，但是javascript实现了类的基本特称——继承的**功能**。

> 在面向对象中，继承被实现在类和类之间，而在js中，继承是在对象（```Object```）中实现的。通俗来讲，在js中，对象```A```和对象```B```之间可以实现继承的**功能**。

总结：**js中没有提供类的实现，只是在对象和对象之间实现了继承的功能**。

- ***原型***

首先我们构造一个简单的原型A
```javascript
  function A(){
    this.a = a
  }
  A.prototype = {
    doSome:function(){
      console.log(this.a)
    }
  }
  A.prototype.constructor = A
  var instanceA = new A(123)
  typeof A // 输出：function
  typeof instanceA // 输出：object
```

> 在js中的继承，也经常可以看到这么一句话
>> js的继承是基于**原型**的。

> 实际上在面型对象性语言中继承基本都是基于类的，换到js上，也就是js的继承是原型和原型之间的。然而，js的原型的```typeof```输出是```function```，通过原型构建的实例```typeof```输出却是```object```。
> 既然不清楚它是对象还是函数，干脆就给它取个新的名字，原型则是代表这种介于```function```和```object```中的结构。

总结：**js的原型指的是自己本身是```function```，却可以含有```prototype```属性的一种结构，另外```prototype```属性中的```constructor```指向本身**。

- ***原型对象prototype***

> 通俗来讲，原型对象是原型中的一个对象，对象的索引为```prototype```。
> js中的对象是可以嵌套对象的，我们也经常使用，例如：
```javascript
const obj = {
  // 对象中的属性也是一个对象
  baseInfo:{
    name:'Jhon',
    age:18,
    desc:'lucky jhon'
  }
}
```
> 原型对象```prototype```是原型的一种特殊属性，浏览器会自动将```prototype```实现为```__proto__```。所以通俗来讲，这两者只是因为浏览器而产生不同的，js标准上还是使用```prototype```

> ***原型链***，顾名思义，其代表的是原型之间的 **联系**。事实上也确实如此，只是我们应该知道这联系的桥梁是```prototype```，详情如下：

```javascript
var a = {a: 1};
// a和Object存在联系：a ---> Object
var b = Object.create(a);
// a、b、Object之间的联系：b ---> a ---> Object
console.log(b.a); // 1 (继承而来)
// 事实上，Object也是有自己的原型对象prototype，可以通过下面的代码获取
Object.prototype // 输出：{}
Object.prototype.prototype // 输出undefined
Object.prototype.prototype == null //输出：true
```
通过上面的代码，我们可以知道，```Object```的原型对象指的是一个空对象，而空对象的原型是```null```，所以```null```是原型链的终点。但是存在一个问题，当原型的原型是它本身的时候，会发生什么呢？代码如下：
```javascript
  var a = {};
  a.prototype = a // 输出：{ prototype: [Circular] }
```

总结：**js原型对象```prototype```实际上就是原型的一种特殊属性，而```__proto__```是浏览器对原型的特殊属性```prototype```的实现。而原型链则是继承实现的方法，换句话说js继承是基于原型而通过原型链实现的**。

---
## 中篇
---

通过***开篇***的介绍，我们对三者的含义都有所了解，开篇将开始对三者关系的梳理。

现在我们所梳理的概念有：
1. js继承是只是实现面向对象中继承的功能
2. js继承是基于原型的
3. js继承是通过原型链实现的

原型链实现继承代码如下：
```javascript
function A(a){
  this.a = a
}

A.prototype = {
  doSome:function(){
    console.log(a);
  }
}
A.prototype.constructor = A
// 到这里我们已经创建了原型A了,我们再构建一个原型B，并使得B继承A
function B(a,b){
  A.call(this, a);
  this.b = b
}
// 再原型B和A之间建立原型链
B.prototype = A.prototype
// B.prototype = Object.create(A.prototype)
B.prototype.constructor = B
```

总结：**js继承是在原型和原型之间构建原型链实现的**

---
## 终篇
---
参考文献：

[继承与原型链——MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

[undefined与null的区别——阮一峰](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)
