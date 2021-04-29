# js-learning

## 继承

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
console.log(s1.name)
console.log(s1.getName())
```
