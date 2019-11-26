# JavaScript-Inherit
JavaScript继承方式  

`子类继承父类的属性，方法。`  
```
<!-- 父类 -->
function Father(name){  //给构造函数添加参数
    this.name = name
    this.sum = function() {
        alert(this.name)
    }
}
Father.prototype.age = 10 //给构造函数添加原型属性
```
**一、原型链继承**
```
function Child() {
    this.name = 'ker'
}
Child.prototype = new Father() //让新实例的原型等于父类的实例
var child1 = new Child()
console.log(child1.age)  // 10 (继承父类age属性)
console.log(child1 instanceof Father) // true

**特点**
1，实例可继承的属性包括：实例的构造函数的属性、父类构造函数的属性、父类原型的属性。
**缺点**
1，新实例无法向父类构造函数传参；
2，所有实例会共享父类实例的属性；

```
---
**二、构造函数继承**
```
function Boy() {
    Father.call(this,'cool') // 父类函数在子类函数里自执行
    this.age = 12
}
var boy1 = new Boy()
console.log(boy1.name) // cool
console.log(boy1.age) // 12
console.log(boy1 instanceof Father)  // false
**特点**
1，只继承父类构造函数的属性，不继承父类原型的属性；
2，解决了原型链继承的缺点；
3，可以继承多个构造函数属性（call多个）；
**缺点**
1，只能继承父类构造函数的属性；
2，构造函数无法复用；
3，每个新实例都有父类构造函数的副本。
```
**三、组合继承（组合原型链继承和构造函数继承）**
```
function Girl(name){
    Father.call(this,name)
}
Girl.prototype = new Father()
var girl1 = new Girl('susan')
console.log(girl1.name) // susan
console.log(girl1.age) // 10
**特点**
1，弥补上面两种继承的缺点；
2，每个新实例引入的构造函数属性是私有的；
**缺点**
1，调用了两次父类构造函数，内存消耗
```
**四、原型式继承**
```
//封装一个函数容器，用来输出对象和承载继承的原型
function content(obj) {
    function F() {}
    F.prototype = obj
    return new F()
}
var sub = new Father()
var sub1 = content(sub)
**特点**
1，类似用函数来包装一个复制的对象
**缺点**
1、所有实例都会继承原型上的属性；
2、无法实现复用
```
**五、寄生式继承**
```
function content(obj) {
    function F(){}
    F.prototype = obj;
    return new F()
}
var sub = new Father()
// 以上是原型式继承   给原型式继承再套个壳子传递参数
function subObject(obj) {
    var sub = content(obj)
    sub.name = 'lisi'
    return sub
}
var sub2 = subObject(sub)
**特点**
1，就是给原型式继承外面套了个壳子
**缺点**
1，无法复用
```
**六、寄生组合式继承**
```
function content(obj) {
    function F(){}
    F.prototype = obj;
    return new F()
}
var con = content(Father.prototype)

var Sub() {
    Person.call(this)
}
Sub.prototype = con
con.constructor = Sub  // 修复实例

var sub1 = new Sub()

**特点**
1，修复了组合继承的问题
```

**六、寄生组合式继承**
```
function content(obj) {
    function F(){}
    F.prototype = obj;
    return new F()
}
var con = content(Father.prototype)

var Sub() {
    Person.call(this)
}
Sub.prototype = con
con.constructor = Sub  // 修复实例

var sub1 = new Sub()

**特点**
1，修复了组合继承的问题
```
**七、es6关键字实现继承**
```
class Student {
    constructor(name) {
        this.name = name
    }
    hello() {
        alert('hello,' + this.name + '!')
    }
}
//关键字 extends 实现继承
class subStudent extends Student {
    constructor(name,grade) {
        super(name) // 一定要用super调用父类构造方法
        this.grade = grade
    }
    myGrade() {
        alert('my grade is' + this.grade)
    }
    
}
```