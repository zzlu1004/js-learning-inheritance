### 1、原型链继承

```
function Parent() {
    this.name = 'Parent'
    this.play = [1, 2, 3]
}
function Child() {
    this.type = 'Child'
}
this.Child.prototype = new Parent()
console.dir(new Child())

const s1 = new Child()
const s2 = new Child()
s1.play.push(4)
console.log(s1.play, s2.play)

```
![Image text](https://github.com/zzlu1004/js-learning/blob/main/images/1-%E5%8E%9F%E5%9E%8B%E9%93%BE%E7%BB%A7%E6%89%BF.png)
#### 缺点：
多个实例指向同一个父类原型对象，修改实例s1中原型对象的某个属性时，另一个也会跟着发生改变。

### 2、构造函数继承（借用call）

```
function Parent() {
    this.name = 'Parent'
}
Parent.prototype.getName = function() {
    return this.name
}
function Child() {
    Parent.call(this)
    this.type = 'Child'
}

let s1 = new Child()
console.log(s1)
console.log(s1.name)
console.log(s1.getName())
```
![Image text](https://github.com/zzlu1004/js-learning/blob/main/images/2-%E5%80%9F%E5%8A%A9call%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E7%BB%A7%E6%89%BF.png)
#### 缺点：
子类无法继承父类通过prototype新增的属性。

### 3、组合继承

```
function Parent() {
    this.name = 'Parent'
    this.play = [1, 2, 3]
}
Parent.prototype.getName = function() {
    return this.name
}
function Child() {
    // 第二次执行 Parent()
    Parent.call(this)
    this.type = 'Child'
}
// 第一次执行 Parent()
Child.prototype = new Parent()
Child.prototype.constructor = Child

const s1 = new Child()
const s2 = new Child()
s1.play.push(4)
console.log(s1.play, s2.play)
console.log(s1.getName())
console.log(s2.getName())
```
![Image text](https://github.com/zzlu1004/js-learning/blob/main/images/3-%E7%BB%84%E5%90%88%E7%BB%A7%E6%89%BF.png)
#### 缺点：
父类funtion Parent()执行了二次，虽解决以上两种继承方式的缺点，但仍不是最优方案。

### 4、原型式继承
```
const parent = {
    name: 'parent',
    friends: [1 , 2, 3],
    getName: function() {
        return this.name
    }
}
const child1 = Object.create(parent)
child1.name = 'tom'
child1.friends.push('jerry')

const child2 = Object.create(parent)
child1.friends.push('lili')

console.log(child1.name)
console.log(child1.name === child1.getName())
console.log(child2.name)
console.log(child1.friends)
console.log(child2.friends)
```
![Image text](https://github.com/zzlu1004/js-learning/blob/main/images/4-%E5%8E%9F%E5%9E%8B%E5%BC%8F%E7%BB%A7%E6%89%BF.png)
#### 缺点：
Object.create为浅拷贝，当父类中属性存在引用类型时，通过child1修改父类中某引用类型属性时，则child2调用该属性值时，也会被篡改。

### 5、寄生式继承
```
const parent = {
    name: 'parent',
    friends: [1, 2, 3],
    getName: function() {
        return this.name
    }
}
function clone(original) {
    const clone = Object.create(parent)
    clone.getFriends = function() {
        return this.friends
    }
    return clone
}

const child1 = clone(parent)
const child2 = clone(parent)
child1.friends.push('lili')

console.log(child1.getName())
console.log(child1.getFriends())
console.log(child2.getFriends())
```
![Image text](https://github.com/zzlu1004/js-learning/blob/main/images/5-%E5%AF%84%E7%94%9F%E5%BC%8F%E7%BB%A7%E6%89%BF.png)
#### 缺点：
新增getFriends属性，缺点同原型式继承一样；只不过在实现方式上有所不同。

### 6、寄生组合式继承
```
function clone(parent, child) {
    child.prototype = Object.create(parent.prototype)
    child.prototype.constructor = child
}
function Parent() {
    this.name = 'Parent'
    this.play = [1, 2, 3]
}
Parent.prototype.getName = function() {
    return this.name
}
function Child() {
    Parent.call(this)
    this.friends = 'lili'
}
clone(Parent, Child)

Child.prototype.getFriends = function() {
    return this.friends;
}

const s1 = new Child()

console.log(s1)
console.log(s1.getName())
console.log(s1.getFriends())
```
![Image text](https://github.com/zzlu1004/js-learning/blob/main/images/6-%E5%AF%84%E7%94%9F%E7%BB%84%E5%90%88%E5%BC%8F%E7%BB%A7%E6%89%BF.png)
#### 总结：
目前最优继承方式

### 7、es6的extends继承
```
class Parent {
    constructor(name) {
        this.name = name
    }
    getName() {
        return this.name
    }
}

class Child extends Parent {
    constructor(name, age) {
        super(name)
        this.age = age
    }
}

const s1 = new Child('lili', 20)

console.log(s1.getName()) // 'lili'
```
#### 总结：
extends语法糖本质上，也是采用寄生组合式继承实现。

