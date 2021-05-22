# 类

## 类的由来

在es6中，javascript终于和java，c++一样引入了类的概念，但是在es6之前，我们也能生成对象模板，那就是通过构造函数。

下面让我们来具体感受一下通过类和构造函数生成对象的不同。

```javascript
    function Person(name,a) {
      this.name = name
    }
    Person.prototype.eat = function () {
        console.log(this.name + '在吃饭');
      }
    let p = new Person('tony')
    p.eat() //tony在吃饭
    let p1 = new Person('sunny')
    p1.eat() //sunny在吃饭
```

我们可以看到我们通过构造函数实例化了两个对象。

```javascript
    class Person {
      constructor(name) {
        this.name = name
      }
      eat() {
        console.log(this.name + '在吃饭');
      }
    }
    let p = new Person('tony')
    p.eat()
    let p1 = new Person('sunny')
    p1.eat()
```

这是我们用class创造了同样的两个对象，那他们有什么不同没有。

## 类的本质

我们可以通过代码打印calss属性查看一下。

```javascript
console.dir(Person)
```

![](https://gitee.com/tianhouju/myimgs/raw/master/img/class1.png)

![](https://gitee.com/tianhouju/myimgs/raw/master/img/class2.png)

我们分别打印了二者的属性，我们可以看到class里面也有prototype，也有constructor，所以我们可以把class看做一种语法糖，用class写法，和我们原来用构造函数的写法没有什么区别，只是，让我们觉得更加清晰，更加像面向过程的语法。

```javascript
    console.log(Person===Person.prototype.constructor);//true
    console.log(p.eat===Person.prototype.eat);//true
```

通过这两行代码，也能印证我们前面所说的话。我们也可以发现定义在类中的方法，是直接定义在了prototype上，我们发现类里面也有个constructor，但是这并不是必须的，constructor是为了，方便我们接收参数的。

## getter和setter

在类的内部，我们也可以设置访问器，来实现对读取和设置属性的行为进行拦截。

```javascript
    class Person {
      constructor(name) {
        this.name = name
    
      }
      eat() {
        console.log(this.name + '在吃饭');
      }
      set height(value){
        console.log(value+10);
      }
      get height(){
          return console.log(188);
      }
    }
    let p = new Person('tony')
    p.height=180 //190
    p.height;   //188
```

## 静态方法

我们的构造函数时一个函数，但是也是一个对象，是对象，我们就能在它身上添加一些属性。

```javascript
    function Person(name) {
      this.name = name
    }
    Person.prototype.eat = function () {
        console.log(this.name + '在吃饭');
      }
    Person.sleep=function(name){
      console.log(name+'在睡觉'); //猪在睡觉
    }
    Person.sleep('猪')
```

我们添加了一个sleep的方法，但是这个方法不是定义在构造函数内部，也不是在原型对象上，而是直接加在了构造函数身上.

![](https://gitee.com/tianhouju/myimgs/raw/master/img/class4.png)

我们可以看到，sleep方法是Person的一个属性，而eat方法，则是在prototype里面，所以这个sleep方法也不会被实例对象所继承。

```javascript
    class Person {
      constructor(name) {
        this.name = name
      }
      eat() {
        console.log(this.name + '在吃饭');
      }
      static sleep(name){
        console.log(name+'在睡觉');
      }
    }
    let p = new Person('tony')
    Person.sleep('猪')
```

我们在类中的定义的方法加上static关键字，也能达到和前面一样的效果，这个方法也无法被实例化出来，我们要调用的话，只有通过Person来调用。

## 私有方法和私有属性

## 类的继承

我们通过前面知道了类实际上是一种语法糖，所以类的继承本质上，也是通过原型和原型链来实现的。所以让我们来回顾一下es5中实现继承

```javascript
       function Phone(number,brand){
           this.number=number
           this.brand=brand
       }
       Phone.prototype.message=function(){
           console.log('发信息');
       }
       function SmartPhone(number,brand){
           Phone.call(this,number,brand)
       }
       SmartPhone.prototype=new Phone()
       SmartPhone.prototype.constructor=SmartPhone
       SmartPhone.prototype.game=function(){
           console.log('玩游戏');
       }
```

通过上面我们就实现了es5中最经典的继承。下面我们用类的继承来重写一下这段代码。

```javascript
        class Phone {
            constructor(number, brand) {
                this.number = number
                this.brand = brand
            }
            message() {
                console.log('发信息');
            }
        }
        class SmartPhone extends Phone {
            constructor(number, brand) {
                super(number, brand)
            }
            game() {
                console.log('玩游戏');
            }
        }
        let vivo=new SmartPhone(120,'vivo')
        console.log(vivo);
```

我们实现了同样的效果。但是语法更为简单，也不要对原型对象，构造函数等进行手动改变