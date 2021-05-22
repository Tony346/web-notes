# This关键字

​       this是js中非常重要的一个语法点，因为环境的不同，其常常能代表不同的涵义，如果用一句话来概括。**`this`就是属性或者方法当前所在的对象**。

在以下场合里面，this的使用非常频繁，而且让人琢磨不透

## 全局环境

```javascript
function f(){
    console.log(this===window) //true
}
f()

console.log(this) //true
```

通过上面的代码我们可以看到，在全局环境中，this的指向都是顶层对象`window`。

## 构造函数

```javascript
var Obj=function(p){
    this.p=p
}
var o=new Obj('hello world')
o.p
```

new关键字的创建实例化对象的过程中，其中就有一步是把this指向实例化的对象。所以这里的this指向o。

## 对象方法

```javascript
var obj={
    fn:function (){
        console.log(this)
    }
}
var obj1={
    
}
obj1.fn1=obj.fn
obj.fn() //object
obj1.fn1()//object
```

​     我们这里打印出来的this指向的就是fn所在的对象obj。此时如果我们把fn赋予一个新的对象，那么obj1的fn1就会打印出obj1。如果某个方法在多个对象之中，那么this又会指向那里呢

```javascript
        var a = {
            b: 'hello world',
    c: {
                d: function () {
                    console.log(this.b) //undefined
                }
            }
        }
        a.c.d()
```

​    d这个方法既a对象又在c对象里面，但是我们记住，this指向最后它所处的对象，不会往更外层去寻找，所以这里的this就是指向c。

**但是在对象里面的this并不是一定指向该对象**

- 如果是立即执行函数，里面的this就会指向window而不是this。

```javascript
var obj={
 fn:(function(){
     console.log(this) //window
 })()
}
```

- 使用另一个变量来给函数取别名

```javascript
        var a = {
            b: 'hello world',
    c: {
                d: function () {
                    console.log(this.b) //undefined
                }
            }
        }
        var m=a.c.d
        m()//undefined
```

​     我们把d这个方法赋予给了m，此时就相当于在全局中定义了一个m方法，this就指向了window

```javascript
        var b='hello'
      var a = {
            b: 'hello world',
    c: {
                d: function () {
                    console.log(this.b) //undefined
                }
            }
        }
        var m=a.c.d
        m()//hello
```

- 将函数作为参数传递时会被隐式赋值，this指向会改变

```javascript
         function foo(){
             console.log(this.a);//1
         }
         function dofoo(fn){
             console.log(this); //{a: 3, dofoo: ƒ}
             fn()
         }
         var a=1
         var obj1={
             a:2,
             foo
         }
         var obj2={
             a:3,
             dofoo
         }
         obj2.dofoo(foo)
```

​     我们将foo作为参数，传递到dofoo里面，传递过程中本来应指向obj1的this会指向window

## call,apoly和bind

call方法，可以知道函数内部this的指向（既函数执行时所在的作用域），然后在知道作用域中，调用该函数

```javascript
     var obj={
         a:1
     }
     function fn1(){
        console.log(this.a);
     }
     fn1()  //undefined
     fn1.call(obj) //1
```

我们可以理解为call方法把全局中的fn1函数，拿到了obj对象里面去。

`call`方法的参数，应该是一个对象。如果参数为空、`null`和`undefined`，则默认传入全局对象。

```javascript
var n = 123;
var obj = { n: 456 };

function a() {
  console.log(this.n);
}

a.call() // 123
a.call(null) // 123
a.call(undefined) // 123
a.call(window) // 123
a.call(obj) // 456
```

call方法也可以接受参数

```javascript
function add(a, b) {
  return a + b;
}

add.call(this, 1, 2) // 3
```

**apply和call唯一的区别就是，apply接收数组作为函数的参数**

```javascript
function add(a, b) {
  return a + b;
}

add.apply(this, [1, 2]) // 3
```

bind会返回一个新的函数，并不会在使用时，自动调用

```javascript
        var counter = {
            count: 0,
            inc: function () {
                this.count++;
            }
        };
         var func = counter.inc.bind(counter);
        func();
        console.log(counter.count);//1
```

