# Object

## assign()

方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。

-   语法：Object.assign(target, ...sources)
-   参数：
    -   target: object 必填 目标对象
    -   sources: object 源对象
-   返回值：object，目标对象。

!> 源对象为 null undefined number boolean sysmbol 被忽略

```javascript
const target = {a: 1, b: 2};
const source = {b: 4, c: 6};
const returnedTarget = Object.assign(target, source);
console.log(target); //Object { a: 1, b: 4, c: 6 }
console.log(source); //{ b: 4, c: 6 }
console.log(returnedTarget); //{ a: 1, b: 4, c: 6 }
console.log(target === returnedTarget); //true
```

## create()

方法创建一个新对象，使用现有的对象来提供新创建的对象的**proto**。

-   语法：Object.create(proto[, propertiesObject])
-   参数：
    -   proto: object 必填 新创建对象的原型对象
    -   propertiesObject: 可选。如果没有指定为 undefined
-   返回值：object，一个新对象，带着指定的原型对象和属性。

!> 如果 propertiesObject 参数是 null 或非原始包装对象，则抛出一个 TypeError 异常

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.say = function() {
    console.log(`Hello  ${this.name}`);
};

function Student(name) {
    Person.call(this, name); // call super constructor.
}

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;

var stu01 = new Student('xiaoming');
console.log(stu01.name); //xiaoming
stu01.say(); //Hello xiaoming
```

## defineProperties()

方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象

-   语法：Object.defineProperties(obj, props)
-   参数：
    -   proto: object 必填 在其上定义或修改属性的对象
    -   props: 要定义其可枚举属性或修改的属性描述符的对象。对象中存在的属性描述符主要有两种：数据描述符和访问器描述符（更多详情，请参阅 Object.defineProperty()）。描述符具有以下键
        -                                                                                                    configurable :true 当且仅当该属性描述符的类型可以被改变并且该属性可以从对应对象中删除🚮。默认为 false
        -                                                                                                    enumerable :true 当且仅当在枚举相应对象上的属性时该属性显现。默认为 false
        -                                                                                                    value:与属性关联的值。可以是任何有效的JavaScript值（数字，对象，函数等）。 默认为 undefined.
        -                                                                                                    writable:true当且仅当与该属性相关联的值可以用assignment operator改变时。默认为 false
        -                                                                                                    get:作为该属性的 getter 函数，如果没有 getter 则为undefined。函数返回值将被用作属性的值。默认为 undefined
        -                                                                                                    set:作为属性的 setter 函数，如果没有 setter 则为undefined。函数将仅接受参数赋值给该属性的新值。默认为 undefined
-   返回值：object，传递给函数的对象。

```javascript
var obj = {};
Object.defineProperties(obj, {
    property1: {
        value: true,
        writable: false
    },
    property2: {
        value: 'Hello',
        writable: false
    }
});
```

## defineProperty()

方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象

-   语法：Object.defineProperty(obj, prop, descriptor)
-   参数：
    -   obj: object 要在其上定义属性的对象。
    -   prop: 要定义或修改的属性的名称。
    -   descriptor: 将被定义或修改的属性描述符。
-   返回值：object，被传递给函数的对象。

?>在 ES6 中，由于 Symbol 类型的特殊性，用 Symbol 类型的值来做对象的 key 与常规的定义或修改不同，而 Object.defineProperty 是定义 key 为 Symbol 的属性的方法之一

### 属性描述符

对象里目前存在的属性描述符有两种主要形式：**数据描述符**和**存取描述符**。**数据描述符**是一个具有值的属性，该值可能是可写的，也可能不是可写的。**存取描述符**是由 getter-setter 函数对描述的属性。描述符必须是这两种形式之一；**不能同时是两者**

**数据描述符**和**存取描述符**均具有以下可选键值(默认值是在使用 Object.defineProperty()定义属性的情况下)：

-                                                                                                    configurable :当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。默认为 false。
-                                                                                                    enumerable :当且仅当该属性的enumerable为true时，该属性才能够出现在对象的枚举属性中。默认为 false。

数据描述符同时具有以下可选键值：

-                                                                                                    value:与属性关联的值。可以是任何有效的JavaScript值（数字，对象，函数等）。 默认为 undefined.
-                                                                                                    writable:true当且仅当与该属性相关联的值可以用assignment operator改变时。默认为 false

存取描述符同时具有以下可选键值：

-                                                                                                    get:作为该属性的 getter 函数，如果没有 getter 则为undefined。函数返回值将被用作属性的值。默认为 undefined
-                                                                                                    set:作为属性的 setter 函数，如果没有 setter 则为undefined。函数将仅接受参数赋值给该属性的新值。默认为 undefined

| type 常数  | configurable | enumerable | value | writable | get | set |
| ---------- | :----------: | :--------: | ----- | -------- | --- | --- |
| 数据描述符 |     Yes      |    Yes     | Yes   | Yes      | No  | No  |
| 数据描述符 |     Yes      |    Yes     | No    | No       | Yes | Yes |

?> 如果一个描述符不具有 value,writable,get 和 set 任意一个关键字，那么它将被认为是一个数据描述符。如果一个描述符同时有(value 或 writable)和(get 或 set)关键字，将会产生一个异常

```javascript
var o = {}; // 创建一个新对象

// 在对象中添加一个属性与数据描述符的示例
Object.defineProperty(o, 'a', {
    value: 37,
    writable: true,
    enumerable: true,
    configurable: true
});
```

## entries()

返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环也枚举原型链中的属性）。

-   语法：Object.entries(obj)
-   参数：
    -   obj: object 可以返回其可枚举属性的键值对的对象
-   返回值：给定对象自身可枚举属性的键值对数组。

```javascript
const object1 = {
    a: 'somestring',
    b: 42
};
let obj = Object.entries(object1);
console.log(obj); //[["a", "somestring"],"b", 42]]
```

## freeze()

冻结一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。

-   语法：Object.freeze(obj)
-   参数：
    -   obj: object 要被冻结的对象
-   返回值：被冻结的对象

!> 冻结的对象属性值是对象时，该属性依旧可以被修改，除非它也是个冻结对象

```javascript
const obj = {
    prop: 42
};
let newObj = Object.freeze(obj);
obj.prop = 33;
newObj.prop = 44;
console.log(obj.prop); //42
console.log(newObj.prop); //42

//obj
const obj = {
    prop: {name: 22}
};
Object.freeze(obj);
obj.prop.name = 33;
console.log(obj.prop.name); //33
```

## fromEntries()

把键值对列表转换为一个对象

-   语法：Object.fromEntries(iterable)
-   参数：
    -   iterable: 迭代对象，类似 Array 、 Map 或者其它实现了可迭代协议的对象
-   返回值：一个由该迭代对象条目提供对应属性的新对象。

?> 迭代对象:第一个元素是将用作属性键的值，第二个元素是与该属性键关联的值,其他的忽略

```javascript
const arr = [['0', 'a', 'c'], ['1', 'c', 'b', '4', 'F'], ['2']];
const obj = Object.fromEntries(arr);
console.log(obj); // { 0: "a", 1: "c", 2: undefined }
```

## getOwnPropertyDescriptor()

返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）

-   语法：Object.getOwnPropertyDescriptor(obj, prop)
-   参数：
    -   obj: 需要查找的目标对象
    -   prop: 目标对象内属性名称
-   返回值：如果指定的属性存在于对象上，则返回其属性描述符对象（property descriptor），否则返回 undefined。

?> 在 ES5 中，如果该方法的第一个参数不是对象（而是原始类型），那么就会产生出现 TypeError。而在 ES2015，第一个的参数不是对象的话就会被强制转换为对象。

```javascript
var o = {
    get foo() {
        return 17;
    }
};
var d = Object.getOwnPropertyDescriptor(o, 'foo');
// d {
//   configurable: true,
//   enumerable: true,
//   get: /*the getter function*/,
//   set: undefined
// }
```

## getOwnPropertyDescriptors()

方法用来获取一个对象的所有自身属性的描述符。

-   语法：Object.getOwnPropertyDescriptors(obj)
-   参数：
    -   obj: 需要查找的目标对象
-   返回值：所指定对象的所有自身属性的描述符，如果没有任何自身属性，则返回空对象

```javascript
Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));
```

## getOwnPropertyNames()

返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括 Symbol 值作为名称的属性）组成的数组

-   语法：Object.getOwnPropertyNames(obj)
-   参数：
    -   obj: 要返回 Symbol 属性的对象
-   返回值：给定对象自身上找到的所有 Symbol 属性的数组

?>Object.getOwnPropertyNames() 返回一个数组，该数组对元素是 obj 自身拥有的枚举或不可枚举属性名称字符串。 数组中枚举属性的顺序与通过 for...in 循环（或 Object.keys）迭代该对象属性时一致。数组中不可枚举属性的顺序未定义

```javascript
var arr = ['a', 'b', 'c'];
console.log(Object.getOwnPropertyNames(arr).sort()); // ["0", "1", "2", "length"]

// 类数组对象
var obj = {0: 'a', 1: 'b', 2: 'c'};
console.log(Object.getOwnPropertyNames(obj).sort()); // ["0", "1", "2"]
```

## getOwnPropertySymbols()

返回一个给定对象自身的所有 Symbol 属性的数组。

-   语法：Object.getOwnPropertySymbols(obj)
-   参数：
    -   obj: 需要查找的目标对象
-   返回值：所指定对象的所有自身属性的描述符，如果没有任何自身属性，则返回空对象

!> 因为所有的对象在初始化的时候不会包含任何的 Symbol，除非你在对象上赋值了 Symbol 否则 Object.getOwnPropertySymbols()只会返回一个空的数组。

```javascript
var obj = {};
var a = Symbol('a');
var b = Symbol.for('b');

obj[a] = 'localSymbol';
obj[b] = 'globalSymbol';

var objectSymbols = Object.getOwnPropertySymbols(obj);

console.log(objectSymbols.length); // 2
console.log(objectSymbols); // [Symbol(a), Symbol(b)]
console.log(objectSymbols[0]); // Symbol(a)
```

## getPrototypeOf()

返回指定对象的原型（内部[[Prototype]]属性的值）

-   语法：Object.getPrototypeOf(obj)
-   参数：
    -   obj: 要返回其原型的对象
-   返回值：给定对象的原型。如果没有继承属性，则返回 null

```javascript
const prototype1 = {};
const prototype2 = {};
const object1 = Object.create(prototype1);

console.log(Object.getPrototypeOf(object1) === prototype1); //true
console.log(Object.getPrototypeOf(object1) === prototype2); //false
```

## hasOwnProperty()

返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是是否有指定的键）

-   语法：obj.hasOwnProperty(prop)
-   参数：
    -   obj: 要检测的属性 字符串 名称或者 Symbol。
-   返回值：用来判断某个对象是否含有指定的属性的 Boolean

```javascript
const obj = {1: 'a', 2: 'b', 3: 'c'};
obj.hasOwnProperty('1'); //true
obj.hasOwnProperty(1); //true
```

## isPrototypeOf()

测试一个对象是否存在于另一个对象的原型链上

-   语法：obj.isPrototypeOf(object)
-   参数：
    -   object: 要在该对象的原型链上搜寻
-   返回值：表示调用对象是否在另一个对象的原型链上

```javascript
function Foo() {}
function Bar() {}
function Baz() {}

Bar.prototype = Object.create(Foo.prototype);
Baz.prototype = Object.create(Bar.prototype);

var baz = new Baz();

console.log(Baz.prototype.isPrototypeOf(baz)); // true
console.log(Bar.prototype.isPrototypeOf(baz)); // true
console.log(Foo.prototype.isPrototypeOf(baz)); // true
console.log(Object.prototype.isPrototypeOf(baz)); // true
```

## is()

方法判断两个值是否是相同的值

-   语法：Object.is(value1, value2)
-   参数：
    -   value1: 第一个需要比较的值
    -   value2: 第二个需要比较的值
-   返回值：表示两个参数是否相同的布尔值

Object.is() 判断两个值是否相同。如果下列任何一项成立，则两个值相同：

    两个值都是 undefined
    两个值都是 null
    两个值都是 true 或者都是 false
    两个值是由相同个数的字符按照相同的顺序组成的字符串
    两个值指向同一个对象
    两个值都是数字并且
    都是正零 +0
    都是负零 -0
    都是 NaN
    都是除零和 NaN 外的其它同一个数字
    这种相等性判断逻辑和传统的 == 运算不同，== 运算符会对它两边的操作数做隐式类型转换（如果它们类型不同），然后才进行相等性比较，（所以才会有类似 "" == false 等于 true 的现象），但 Object.is 不会做这种类型转换。
    这与 === 运算符的判定方式也不一样。=== 运算符（和== 运算符）将数字值 -0 和 +0 视为相等，并认为 Number.NaN 不等于 NaN。

```javascript
Object.is('foo', 'foo'); // true
Object.is(window, window); // true

Object.is('foo', 'bar'); // false
Object.is([], []); // false

var foo = {a: 1};
var bar = {a: 1};
Object.is(foo, foo); // true
Object.is(foo, bar); // false

Object.is(null, null); // true

// 特例
Object.is(0, -0); // false
Object.is(0, +0); // true
Object.is(-0, -0); // true
Object.is(NaN, 0 / 0); // true
```

## isExtensible()

判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。

-   语法：Object.isExtensible(obj)
-   参数：
    -   obj: 需要检测的对象
-   返回值：表示给定对象是否可扩展的一个 Boolean
    ?>默认情况下，对象是可扩展的：即可以为他们添加新的属性。以及它们的 **proto** 属性可以被更改。Object.preventExtensions，Object.seal 或 Object.freeze 方法都可以标记一个对象为不可扩展（non-extensible）。
    !> 如果参数不是一个对象类型，将抛出一个 TypeError 异常。在 ES6 中， non-object 参数将被视为一个不可扩展的普通对象，因此会返回 false

```javascript
// 新对象默认是可扩展的.
var empty = {};
Object.isExtensible(empty); // === true

// ...可以变的不可扩展.
Object.preventExtensions(empty);
Object.isExtensible(empty); // === false

// 密封对象是不可扩展的.
var sealed = Object.seal({});
Object.isExtensible(sealed); // === false

// 冻结对象也是不可扩展.
var frozen = Object.freeze({});
Object.isExtensible(frozen); // === false
```

## isFrozen()

方法判断一个对象是否被冻结

-   语法：Object.isFrozen(obj)
-   参数：
    -   obj: 被检测的对象
-   返回值：表示给定对象是否被冻结的 Boolean

?>一个对象是冻结的是指它不可扩展，所有属性都是不可配置的，且所有数据属性（即没有 getter 或 setter 组件的访问器的属性）都是不可写的。

!>在 ES5 中，如果参数不是一个对象类型，将抛出一个 TypeError 异常。在 ES2015 中，非对象参数将被视为一个冻结的普通对象，因此会返回 true

```javascript
// 一个对象默认是可扩展的,所以它也是非冻结的.
Object.isFrozen({}); // === false

// 一个不可扩展的空对象同时也是一个冻结对象.
var vacuouslyFrozen = Object.preventExtensions({});
Object.isFrozen(vacuouslyFrozen); //=== true;

// 一个非空对象默认也是非冻结的.
var oneProp = {p: 42};
Object.isFrozen(oneProp); //=== false

// 让这个对象变的不可扩展,并不意味着这个对象变成了冻结对象,
// 因为p属性仍然是可以配置的(而且可写的).
Object.preventExtensions(oneProp);
Object.isFrozen(oneProp); //=== false

// ...如果删除了这个属性,则它会成为一个冻结对象.
delete oneProp.p;
Object.isFrozen(oneProp); //=== true

// 一个不可扩展的对象,拥有一个不可写但可配置的属性,则它仍然是非冻结的.
var nonWritable = {e: 'plep'};
Object.preventExtensions(nonWritable);
Object.defineProperty(nonWritable, 'e', {writable: false}); // 变得不可写
Object.isFrozen(nonWritable); //=== false

// 把这个属性改为不可配置,会让这个对象成为冻结对象.
Object.defineProperty(nonWritable, 'e', {configurable: false}); // 变得不可配置
Object.isFrozen(nonWritable); //=== true

// 一个不可扩展的对象,拥有一个不可配置但可写的属性,则它仍然是非冻结的.
var nonConfigurable = {release: 'the kraken!'};
Object.preventExtensions(nonConfigurable);
Object.defineProperty(nonConfigurable, 'release', {configurable: false});
Object.isFrozen(nonConfigurable); //=== false

// 把这个属性改为不可写,会让这个对象成为冻结对象.
Object.defineProperty(nonConfigurable, 'release', {writable: false});
Object.isFrozen(nonConfigurable); //=== true

// 一个不可扩展的对象,值拥有一个访问器属性,则它仍然是非冻结的.
var accessor = {
    get food() {
        return 'yum';
    }
};
Object.preventExtensions(accessor);
Object.isFrozen(accessor); //=== false

// ...但把这个属性改为不可配置,会让这个对象成为冻结对象.
Object.defineProperty(accessor, 'food', {configurable: false});
Object.isFrozen(accessor); //=== true

// 使用Object.freeze是冻结一个对象最方便的方法.
var frozen = {1: 81};
Object.isFrozen(frozen); //=== false
Object.freeze(frozen);
Object.isFrozen(frozen); //=== true

// 一个冻结对象也是一个密封对象.
Object.isSealed(frozen); //=== true

// 当然,更是一个不可扩展的对象.
Object.isExtensible(frozen); //=== false
```

## isSealed()

判断一个对象是否被密封

-   语法：Object.isSealed(obj)
-   参数：
    -   obj: 要被检查的对象
-   返回值：表示给定对象是否被密封的一个 Boolean

?> 如果这个对象是密封的，则返回 true，否则返回 false。密封对象是指那些不可 扩展 的，且所有自身属性都不可配置且因此不可删除（但不一定是不可写）的对象

```javascript
// 新建的对象默认不是密封的.
var empty = {};
Object.isSealed(empty); // === false

// 如果你把一个空对象变的不可扩展,则它同时也会变成个密封对象.
Object.preventExtensions(empty);
Object.isSealed(empty); // === true

// 但如果这个对象不是空对象,则它不会变成密封对象,因为密封对象的所有自身属性必须是不可配置的.
var hasProp = {fee: 'fie foe fum'};
Object.preventExtensions(hasProp);
Object.isSealed(hasProp); // === false

// 如果把这个属性变的不可配置,则这个对象也就成了密封对象.
Object.defineProperty(hasProp, 'fee', {configurable: false});
Object.isSealed(hasProp); // === true

// 最简单的方法来生成一个密封对象,当然是使用Object.seal.
var sealed = {};
Object.seal(sealed);
Object.isSealed(sealed); // === true

// 一个密封对象同时也是不可扩展的.
Object.isExtensible(sealed); // === false

// 一个密封对象也可以是一个冻结对象,但不是必须的.
Object.isFrozen(sealed); // === true ，所有的属性都是不可写的
var s2 = Object.seal({p: 3});
Object.isFrozen(s2); // === false， 属性"p"可写

var s3 = Object.seal({
    get p() {
        return 0;
    }
});
Object.isFrozen(s3); // === true ，访问器属性不考虑可写不可写,只考虑是否可配置
```

## keys()

返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和使用 for...in 循环遍历该对象时返回的顺序一致 。如果对象的键-值都不可枚举，那么将返回由键组成的数组

-   语法：Object.keys(obj)
-   参数：
    -   obj: 要返回其枚举自身属性的对象
-   返回值：一个表示给定对象的所有可枚举属性的字符串数组

?> Object.keys 返回一个所有元素为字符串的数组，其元素来自于从给定的 object 上面可直接枚举的属性。这些属性的顺序与手动遍历该对象属性时的一致

!>在 ES5 里，如果此方法的参数不是对象（而是一个原始值），那么它会抛出 TypeError。在 ES2015 中，非对象的参数将被强制转换为一个对象。

```javascript
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); //  ['0', '1', '2']
var obj1 = {0: 'a', 1: 'b', 2: 'c'};
console.log(Object.keys(obj1)); //  ['0', '1', '2']
var arr2 = 'hello';
console.log(Object.keys(arr2)); // ["0", "1", "2", "3", "4"]
var arr = null;
console.log(Object.keys(arr)); //Cannot convert undefined or null to object
var arr = true;
console.log(Object.keys(arr)); //[]
```

## preventExtensions()

让一个对象变的不可扩展，也就是永远不能再添加新的属性。

-   语法：Object.preventExtensions(obj)
-   参数：
    -   obj: 将要变得不可扩展的对象。
-   返回值：已经不可扩展的对象。

?> Object.preventExtensions()仅阻止添加自身的属性。但属性仍然可以添加到对象原型。一旦使其不可扩展，就无法再对象进行扩展

```javascript
var obj = {};
var obj2 = Object.preventExtensions(obj);
obj.age = 12;
obj2.name = 'extensions';
console.log(obj); //{}
console.log(obj2); //{}
console.log(obj == obj2); //true
```

## seal()

封闭一个对象，阻止添加新属性并将所有现有属性标记为不可配置。当前属性的值只要可写就可以改变。

-   语法：Object.seal(obj)
-   参数：
    -   obj: 将要被密封的对象.
-   返回值：密封的对象。

?>不会影响从原型链上继承的属性。但 **proto** ( ) 属性的值也会不能修改。

```javascript
const object1 = {
    property1: 42
};
Object.seal(object1);
object1.property1 = 33;
console.log(object1.property1); // expected output: 33
delete object1.property1; // cannot delete when sealed
object1.name = 'seal'; //cannot add  when sealed
console.log(object1); //{ property1: 33 }
```

## setPrototypeOf()

设置一个指定的对象的原型 ( 即, 内部[[Prototype]]属性）到另一个对象或 null。

-   语法：Object.setPrototypeOf(obj,prototype)
-   参数：
    -   obj: 要设置其原型的对象.
    -   prototype: 该对象的新原型(一个对象 或 null).
-   返回值：

?>Object.setPrototypeOf()是 ECMAScript 6 最新草案中的方法，相对于 Object.prototype.**proto** ，它被认为是修改对象原型更合适的方法

```javascript
var dict = Object.setPrototypeOf({}, null);
```

## values()

方法返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用 for...in 循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。

-   语法：Object.values(obj)
-   参数：
    -   obj: 被返回可枚举属性值的对象.
-   返回值：array 一个包含对象自身的所有可枚举属性值的数组。

!>Object.values()返回一个数组，其元素是在对象上找到的可枚举属性值。属性的顺序与通过手动循环对象的属性值所给出的顺序相同

```javascript
var obj = {
    foo: 'bar',
    baz: 42,
    arr: {a: 'aa', b: 'BB'},
    add: () => {
        console.log('add');
    }
};
console.log(Object.values(obj)); //["bar", 42, {a: "aa", b: "BB"}, () => {console.log('add');}]

var objArr = [1, 2, 3];
console.log(Object.values(objArr)); //[1, 2, 3]
```

<!-- 1.  Object.getPrototypeOf //方法返回指定对象的原型（内部[[Prototype]]属性的值） 对对象的属性读写操作
    Object.getPrototypeOf(object)
    obj:要返回其原型的对象
2.  Object.getOwnPropertyDescriptor //方法返回指定对象上一个自有属性对应的属性描述符。（自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性）
    Object.getOwnPropertyDescriptor(obj, prop)
    obj：需要查找的目标对象
    prop：目标对象内属性名称（String 类型）
    如果指定的属性存在于对象上，则返回其属性描述符对象（property descriptor），否则返回 undefined。
3.  Object.getOwnPropertyNames //返回一个数组，该数组对元素是 obj 自身拥有的枚举或不可枚举属性名称字符串 方法返回一个由指定对象的所有自身属性的属性名
    Object.getOwnPropertyNames(obj)
    obj：一个对象，其自身的可枚举和不可枚举属性的名称被返回。
4.  Object.create // 方法会使用指定的原型对象及其属性去创建一个新的对象。深度创建对象副本
    Object.create(proto, [propertiesObject])
    proto：新创建对象的原型对象。
    propertiesObject:可选,如果没有指定为 undefined，则是要添加到新创建对象的可枚举属性（即其自身定义的属性，而不是其原型链上的枚举属性）
    对象的属性描述符以及相应的属性名称。这些属性对应 Object.defineProperties()的第二个参数。
    返回一个新对象，带着指定的原型对象和属性。
5.  Object.defineProperty//方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。
    Object.defineProperty(obj, prop, descriptor)
    obj：要在其上定义属性的对象。
    prop：要定义或修改的属性的名称。
    descriptor：将被定义或修改的属性描述符。
    返回被传递给函数的对象。
6.  Object.defineProperties 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。
    Object.defineProperties(obj, prop, descriptor)
    obj：要在其上定义属性的对象。
    prop：要定义或修改的属性的名称。
    descriptor：将被定义或修改的属性描述符。
    返回：被传递给函数的对象。
7.  Object.seal 方法可以让一个对象密封，并返回被密封后的对象。密封对象将会阻止向对象添加新的属性，并且会将所有已有属性的可配置性（configurable）置为不可配置（false），
    即不可修改属性的描述或删除属性。但是可写性描述（writable）为可写（true）的属性的值仍然可以被修改。
    obj：将要被密封的对象
8.  Object.freeze 方法可以冻结一个对象，冻结指的是不能向这个对象添加新的属性，不能修改其已有属性的值，不能删除已有属性，以及不能修改该对象已有属性的可枚举性、
    可配置性、可写性。也就是说，这个对象永远是不可变的。该方法返回被冻结的对象。
9.  Object.preventExtensions //方法让一个对象变的不可扩展，也就是永远不能再添加新的属性。
10. Object.isSealed //方法判断一个对象是否被密封。
11. Object.isFrozen //方法判断一个对象是否被冻结
12. Object.isExtensible//用于拦截对对象的 Object.isExtensible()
13. Object.keys 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和使用 for...in 循环遍历该对象时返回的顺序一致
    （两者的主要区别是 一个 for-in 循环还会枚举其原型链上的属性）。 -->
