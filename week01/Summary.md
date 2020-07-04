学习笔记

### 1、可以通过知名库贡献PR来增加闪光点
### 2、职业规划：
>工程师、资深工程师、总监

### 3、前端技能模型：
- 领域知识（实践中学习）

- 前端知识（建立知识体系）

- 编程能力、架构能力、工程能力（刻意练习）

### 4、学习方式
- 整理法：将前端知识体系逐一整理出来，可通过w3c或MDN等网站进行查询。

- 追溯法：将某一知识点通过溯源的方式，从最初提出者的代码或者言论，到现在在优秀项目中的使用。通过了解这个技术点的迭代过程，才能更好地深入了解某一个技术点。

### 5、知识点：
#### 闭包：
>1. 密闭的容器，类似于set，map容器，存储数据的
>2. 闭包是一个对象，存放数据的格式：key:value

形成的条件：
>1. 函数嵌套
>2. 内部函数引用外部函数的局部变量

闭包的优点：
>延长外部函数局部变量的生命周期

闭包的缺点：
>使用不当容易造成内存泄漏

注意点：
>1. 合理的使用闭包
>2. 用完闭包要及时清除或者销毁
```
function fun(n, o) {
  // var n = 0
  console.log(o)
  return {
    // 形成闭包
    fun: function(m) {
      // var m;
      return fun(m,n)
    }
  }
}

var a = fun(0); // undifined

a.fun(1); // 0
a.fun(2); // 0
a.fun(3); // 0

// fun(0).fun(1) => window.fun(1, 0); var n = 1, var o = 0
// fun(1, 0).fun(2) => window.fun(2, 1); var n = 2, var o = 1
// fun(2, 1).fun(3) => window.fun(3, 2); var n = 3, var o = 2
// fun(3, 2).fun(10) => window.fun(10, 3); var n = 10, var o = 3
var b = fun(0).fun(1).fun(2).fun(3).fun(10); // undifined 0 1 2 3

var c = fun(0).fun(1); // 0
c.fun(2); // 1
c.fun(3); // 1
```


#### 原型：
```
 function Person(name, age){
    this.name = name;
    this.age = age;
 }

 Person.prototype.motherland = 'china'
 let person01 = new Person('小明', 18);

// 1. Person.prototype.constructor == Person // **准则1：原型对象（即Person.prototype）的constructor指向构造函数本身**
// 2. person01.__proto__ == Person.prototype // **准则2：实例（即person01）的__proto__和原型对象指向同一个地方**

```

#### this指向问题：
>1. 函数被调用，被谁调用，函数中的`this`指向谁。没有调用者指向`window`。
>2. 箭头函数中的`this`是外部作用域的`this`，解决了之前要缓存`this`的弊端。
>3. `DOM`节点绑定事件方法，方法中的`this`指向当前绑定元素。
>4. 构造函数中，`this`绑定到当前创建的对象实例。
>5. 通过`apply`或者`call`的方式可以改变`this`的指向。`bind`也可以改变，但不会立即执行函数。

#### 双向绑定：
> - Model绑定到View，JS代码更新Model，View就会自动更新，反之用户更新了View，Model数据也会自动更新。
> - Vue：Vue内部使用`Object.defineProperty`方法来拦截，把`data`对象的所有数据的读写转化为`getter/setter`，当数据发生变化时通知视图更新。
```
/**
  * 循环遍历数据对象的每个属性
  */
function observable(obj) {
    if (!obj || typeof obj !== 'object') {
        return;
    }
    let keys = Object.keys(obj);
    keys.forEach((key) => {
        defineReactive(obj, key, obj[key])
    })
    return obj;
}
/**
 * 将对象的属性用 Object.defineProperty() 进行设置
 */
function defineReactive(obj, key, val) {
    Object.defineProperty(obj, key, {
        get() {
            console.log(`${key}属性被读取了...`);
            return val;
        },
        set(newVal) {
            console.log(`${key}属性被修改了...`);
            val = newVal;
        }
    })
}

// 监听person
let person = observable({
    name: 'tom',
    age: 15
});

```