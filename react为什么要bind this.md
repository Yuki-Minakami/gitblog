用一句话来解释：使用ES6 class关键字声明的类无法保证类方法中的this永远指向类的实例

下面是一个使用class关键字声明的类：
```
class Person{
    constructor(){
        this.name = "Lear"
    }
    getThis(){
        console.log(this);
    }
}

```

getThis方法除了打印方法体内的this指向外没有做任何事情，下面来比较两段代码

```
var person = new Person();
person.getThis(); // 输出 Person { name: 'Lear' }
```

```
var person = new Person();
var getThis = person.getThis;
getThis(); // 输出 undefined
```
仅仅是把类方法赋值给了一个额外的变量，再去调用就得到了不同的结果，那么如何保证getThis方法中的this永远指向该方法当前所属的实例呢？
答案很简单，就是在class的构造函数中增加一句

```
class Person{
    constructor(){
        this.name = "Lear";
        this.getThis = this.getThis.bind(this); // 增加这一句
    }
    // xxxx
}
```

再看看react中组件类的定义，是不是觉得很眼熟

事实上，在react的渲染的过程中，也确确实实地将类内部的方法赋给了别的变量，例如onChange。

值得注意的是，使用传统prototype方式声明的类稍微有些不同，

```
function Person(){
    this.name = "Lear";
}

Person.prototype.getThis = function(){
    console.log(this);
}

var person = new Person();
person.getThis(); //Person { name: 'Lear' }

var getThis = person.getThis;
getThis(); // global Object

```
要想让这两种调用保持一致，还是需要在Person方法内部显式绑定


