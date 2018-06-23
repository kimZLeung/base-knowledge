### `Number.isNaN`和 `isNaN` 有什么区别

首先我们要明确`NaN`：NaN是一个 value, 这个 value 的 type 是 number

然而我们`window.NaN`方法，对于一些非Number的东西也会返回true，比如

```js
isNaN('A String'); // ture

isNaN(undefined); // ture

isNaN({}); // ture

// 很明显不是 NaN 的 value 也被误判成 NaN 了
```

所以ES6新增`Number.isNaN`，用于更加精确地判断`NaN`

```js
Number.isNaN = function(n) {
    return (
        typeof n === "number" &&
        window.isNaN(n)
    )
}
```





### `Object.getPrototypeOf()` 和 `Reflect.getPrototypeOf()` 有哪些差异

`Reflect.getPrototypeOf`和`Object.getPrototypeOf`的一个区别是，如果参数不是对象，`Object.getPrototypeOf`会将这个参数转为对象，然后再运行，而`Reflect.getPrototypeOf`会报错。



### `Array.from` 与 `Array.of` 有什么区别

前者的参数是类数组或可迭代对象，用于将这些结构转化为数组。

后者则是直接传入一系列参数，将其转为数组



### `charCodeAt` 和 `codePointAt` 有什么区别

JavaScript 中，1个字符固定为2个字节，对于需要4个字节存储的字符会被认为是2个字符，length会判定为2，`charAt()`无法读取整个字符，`charCodeAt()`只能分别返回前后各两个字节的值。

ES6 提供了 codePointAt()，能够正确处理4个字节存储的字符，返回一个字符的码点。但需要注意的是，这并没有改变 JavaScript 将2个字节视为1个字符的事实，只是自动识别出了这是个4字节的字符并返回了正确的码点而已，对于单个4字节的字符来说，charPointAt(0)返回完整字符的十进制码点，charPointAt(1)返回这个字符的后2个字节的十进制码点，效果等同于charCodeAt(1)。



### `fromCharCode` 和 `fromCodePoint` 有什么区别

作用和charPointAt()相反，从四字节码字得到一个完整字符，解决了 ES5 中fromCharCode()只能识别双字节的问题。



### 关于迭代器（iterator）

ES6中有某些数据包含自身的`iterator`，用于`for...of`，遍历，比如说原来的Array，ES6的Set，Map，WeakSet，WeakMap。对于这些数据类型

- 可以通过 `Symbol.iterator` 来访问他们的默认迭代器
- Set和Array默认的`iterator`就相当于他们的`values`方法返回的`iterator`
- map默认的`iterator`就相当于他们的`entries`方法返回的`iterator`



### class语法

用`extends`关键字继承了父类的子类，如果声明构造方法，则需要在构造方法内调用`super()`，不然会无法访问到`this`

如果你没有指定构造函数，`super()`方法也会自动为你调用



```js
let PersonClass = class PersonClass2 {
  //...
};

console.log(typeof PersonClass)
console.log(typeof PersonClass2)

// 'function'
// undefined
```

和function类似



> 声明静态方法直接在class里面使用`static`关键字，声明静态属性可以直接赋值在类上，如`person.xx = xxy`。或者使用`static`关键字。



### 默认参数

```js
function mixArgs(first, second = "b") {
    console.log(arguments.length);
    console.log(first === arguments[0]);
    console.log(second === arguments[1]);
}
mixArgs();

// 0
// undefined
// 'b'
```

默认参数无法影响到`arguments`对象

```js
var {foo=3} = { foo: 2 }
console.log(foo)

var {foo=3} = { foo: undefined }
console.log(foo)

var {foo=3} = { bar: 2 }
console.log(foo)

var [b=10] = [undefined]
console.log(b)
```

当得到的值为`undefined`时，采用默认赋值



### Symbol

```js
let uid3 = Symbol("uid");
console.log(Symbol.keyFor(uid3));
// undefined
```

采用`Symbol.for()`才可以创建复用的Symbol值



### Generator

```js
function *createIterator() {
    yield 1;
    return;
    yield 2;
    yield 3;
}

let iterator = createIterator();

console.log(iterator.next());
console.log(iterator.next());

// {value: 1, done: false}
// {value: undefined, done: true}
```

`return` 之后结束，不会管后续的`yield`



### Array

- Array.from
- Array.of
- [].copyWithin(target, start, end)：该方法改变原数组
- [].find：返回元素
- [].findIndex：返回下标
- [].fill：填充数组（第二、第三个参数可以指定起始和结束位置）
- [].include：第二个参数为查找起始位置





### Number

- Number.isFinite：如果参数类型不是数值，`Number.isFinite`一律返回`false`

- Number.isNaN：如果参数类型不是数值，`Number.isNaN`一律返回`false`

- Number.isInteger：`Number.isInteger(25) === Number.isInteger(25.0)`，因为JavaScript内部，整数和浮点数采用的是同样的储存方法

- Number.EPSILON：可以用来设置“能够接受的误差范围

  ```js
  // 最小误差范围为 Number.EPSILON * 2的2次方，即2的-50次方
  function withinErrorMargin (left, right) {
    return Math.abs(left - right) < Number.EPSILON * Math.pow(2, 2);
  }
  ```

- Number.MAX_SAFE_INTEGER：JS能表示的最大值

- Number.MIN_SAFE_INTEGER：JS能表示的最小值

- Number.isSafeInteger：是用来判断一个整数是否落在这个范围之内。





### String

- ''.codePointAt：和charCodeAt有点相似，不过这个函数可以将占4位的字码正确返回
- ''.includes：第二个参数指定起始搜索位置
- ''.startsWith：第二个参数指定第几位开始start
- ''.endsWith：第二个参数指定第几位为end
- ''.repeat：表示使用实例字符串重复几次，传入的参数表示重复的次数，向下取整
- ''.padStart：头部补全，第一个参数用来指定字符串的最小长度
- ''.padEnd：尾部补全，第一个参数用来指定字符串的最小长度




### Proxy

用法

```js
var person = {
  name: "张三"
};

var proxy = new Proxy(person, {
  get: function(target, property) {
    if (property in target) {
      return target[property];
    } else {
      throw new ReferenceError("Property \"" + property + "\" does not exist.");
    }
  }
});

proxy.name // "张三"
proxy.age // 抛出一个错误
```

可以通过proxy一个对象，来拦截本来通过对象所做的一些操作

值得注意的是：通过`Proxy`去代理返回的对象去访问原对象的方法时，内部的`this`值指向会发生变化




### Reflect

- Reflect.apply(target, thisArg, args)
- Reflect.construct(target, args)
- Reflect.get(target, name, receiver)
- Reflect.set(target, name, value, receiver)
- Reflect.defineProperty(target, name, desc)
- Reflect.deleteProperty(target, name)
- Reflect.has(target, name)
- Reflect.ownKeys(target)
- Reflect.isExtensible(target)
- Reflect.preventExtensions(target)
- Reflect.getOwnPropertyDescriptor(target, name)
- Reflect.getPrototypeOf(target)
- Reflect.setPrototypeOf(target, prototype)

一共13个静态方法，大部分与`Object`对象的同名方法的作用都是相同的。

> 这个对象出现的目的是

1. 将`Object`对象的一些明显属于语言内部的方法（比如`Object.defineProperty`），放到`Reflect`对象上
2. 修改某些`Object`方法的返回结果，让其变得更合理。
3. 让`Object`操作都变成函数行为
4. `Reflect`对象的方法与`Proxy`对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。也就是说，不管`Proxy`怎么修改默认行为，你总可以在`Reflect`上获取默认行为。



### Async函数

- Async函数返回Promise对象
- Async返回的Promise对象。必须等到内部所有`await`命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到`return`语句或者抛出错误。
- await命令后面不是接着一个Promise对象时，会把它用Promise.resolve()返回。
- await命令后面的Promise一旦reject，整个`async`函数都会中断执行。并且可以在后续的catch函数中捕获到reject的参数，但是可以通过`try...catch`在`Async`函数内部捕获，因为这就不会给机会自动执行器去捕获并`reject`掉整个`Promise`。
- 这和实现原理相关，因为上一个Promise是reject的话，下一个不会再继续`next`，而是直接`throw`

```js
// 类似Generator的自动执行器
function spawn (gen) {
	return new Promise((res, rej) => {
		const g = gen()
		function step (nextF) {
			let next
			try {
				next = nextF()
			} catch (e) {
				rej(e)
			}
			if (next.done === true) {
				return res(next.value)
			}
			Promise.resolve(next.value).then((v) => {
				step(function () { return g.next(v) })
			}, (e) => {
				step(function () { return g.throw(e) })
			})
		}
		step(() => { return g.next(undefined) })
	})
}
```





### Generator

- `Generator.prototype.next()`
- `Generator.prototype.throw()`：throw语句可以在`Generator`函数内部被捕获，就算`Generator`已经执行完毕，还是可以抛出错误
- `Generator.prototype.return()`：如果`Generator`函数内部有`try...finally`语句，return语句需要等到finally代码块执行完才能return。就算`Generator`已经执行完毕，还是可以返回值，而且done为true

这三个方法实际上在做同一件事情，就是恢复`Generator`函数的执行，并且使用语句替换掉`yield`语句

```js
// next
const g = function* (x, y) {
  let result = yield x + y;
  return result;
};

const gen = g(1, 2);
gen.next(); // Object {value: 3, done: false}

gen.next(1); // Object {value: 1, done: true}
// 相当于将 let result = yield x + y
// 替换成 let result = 1;

// throw
gen.throw(new Error('出错了')); // Uncaught Error: 出错了
// 相当于将 let result = yield x + y
// 替换成 let result = throw(new Error('出错了'));

gen.return(2); // Object {value: 2, done: true}
// 相当于将 let result = yield x + y
// 替换成 let result = return 2;
```



`yield*`后面跟一个可遍历的对象，比如`Generator`执行返回的对象，或者数组等原生部署有`[Symbol.iterator]`的对象。作用是可以通过一次又一次的`next`把可遍历对象拆开

`Generator`函数内部的`this`值指向（不知道指向哪里），但是一定不会指向由它生成的可遍历对象，而且不能通过new调用`Generator`函数，所以在`Generator`里面指定this赋值并没有什么用。



### Decorator

```js
@decorator
class A {}

// 等同于

class A {}
A = decorator(A) || A;
```

修饰器对类的行为的改变，是代码编译时发生的，而不是在运行时



方法的修饰

```
function readonly(target, name, descriptor){
  // descriptor对象原来的值如下
  // {
  //   value: specifiedFunction,
  //   enumerable: false,
  //   configurable: true,
  //   writable: true
  // };
  descriptor.writable = false;
  return descriptor;
}
```


### 箭头函数

> this绑定为词法作用域上的this

（1）函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。

- `this`指向的固定化，并不是因为箭头函数内部有绑定`this`的机制，实际原因是箭头函数根本没有自己的`this`，导致内部的`this`就是外层代码块的`this`

（2）不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。

（3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。



### 尾调用

> 函数调用会在内存形成一个“调用记录”，又称“调用帧”（call frame），保存调用位置和内部变量等信息。如果在函数`A`的内部调用函数`B`，那么在`A`的调用帧上方，还会形成一个`B`的调用帧。等到`B`运行结束，将结果返回到`A`，`B`的调用帧才会消失。如果函数`B`内部还调用函数`C`，那就还有一个`C`的调用帧，以此类推。所有的调用帧，就形成一个“调用栈”（call stack）。

通过`return innerFn()`的方式在外部函数调用内部函数，因为此时外部函数已进行`return`操作，所以可以直接直接用内层函数的调用帧，取代外层函数的调用帧就可以了。