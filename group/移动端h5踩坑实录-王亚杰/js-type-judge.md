## 关于类型判断：

###### 1、typeof 

typeof只能判断简单类型不能判断符合类型，typeof操作符返回一个字符串，表示未经计算的操作数的类型，无法识别正则表达式。具体表现如下：

（1）数字  'number'

```
typeof 0 = 'number';typeof NaN = 'number';
```

 （2）字符串  'string'

```
typeof "aaa" = 'string';
```

 （3）布尔值  'boolean'
 
```
typeof true = 'boolean';
```

（4）对象 、数组 、null，Date对象, 'object'

```
typeof {} = "object"; 
typeof [] = "object";
typeof null = "object";
typeof new Date() = "object";
```
（5）函数  'function'

```
typeof function(){} = "function";
```

（6）undefined  'undefined '

```
typeof undefined = 'function';
```
(7)Symbol  'symbol'

```
typeof Symbol() = 'symbol';
```

###### 2、instanceof
a instanceof B 是检测B.prototype类型是不是存在于a的原型链上，主要用于判断继承。
undefined 和 null 没有原型链，不能用instanceof
 
```
1 instanceof Number; //false
new Number(1) instanceof Number; //true

"aaa" instanceof String; //false
new String("aaa") instanceof String; //true

true instanceof Boolean; //false
new Boolean(false) instanceof Boolean;  //true


let fun = function(){};
fun instanceof Function; // true

```
模拟instanceof

```
function _instanceof(A, B) {
    var O = B.prototype;// 取B的显示原型
    A = A.__proto__;// 取A的隐式原型
    while (true) {
        //Object.prototype.__proto__ === null
        if (A === null)
            return false;
        if (O === A)// 这里重点：当 O 严格等于 A 时，返回 true
            return true;
        A = A.__proto__;
    }
}
```

继承中判断实例是否属于它的父类

```
function Person(){};
function Student(){};
var p =new Person();
Student.prototype=p;//继承原型
var s=new Student();
console.log(s instanceof Student);//true
console.log(s instanceof Person);//true
```


复杂用法

```
function Person() {}
console.log(Object instanceof Object);     //true
//第一个Object的原型链：Object=>
//Object.__proto__ => Function.prototype=>Function.prototype.__proto__=>Object.prototype
//第二个Object的原型：Object=> Object.prototype

console.log(Function instanceof Function); //true
//第一个Function的原型链：Function=>Function.__proto__ => Function.prototype
//第二个Function的原型：Function=>Function.prototype

console.log(Function instanceof Object);   //true
//Function=>
//Function.__proto__=>Function.prototype=>Function.prototype.__proto__=>Object.prototype
//Object => Object.prototype

console.log(Person instanceof Function);      //true
//Person=>Person.__proto__=>Function.prototype
//Function=>Function.prototype

console.log(String instanceof String);   //false
//第一个String的原型链：String=>
//String.__proto__=>Function.prototype=>Function.prototype.__proto__=>Object.prototype
//第二个String的原型链：String=>String.prototype

console.log(Boolean instanceof Boolean); //false
//第一个Boolean的原型链：Boolean=>
//Boolean.__proto__=>Function.prototype=>Function.prototype.__proto__=>Object.prototype
//第二个Boolean的原型链：Boolean=>Boolean.prototype

console.log(Person instanceof Person); //false
//第一个Person的原型链：Person=>
//Person.__proto__=>Function.prototype=>Function.prototype.__proto__=>Object.prototype
//第二个Person的原型链：Person=>Person.prototype
```
###### 3、Object.prototype.toString.call

```
Object.prototype.toString.call(1); //"[object Number]"
Object.prototype.toString.call(NaN); //"[object Number]"
Object.prototype.toString.call(new Number(1)); //"[object Number]"

Object.prototype.toString.call("222"); //"[object String]"

Object.prototype.toString.call(true); //"[object Boolean]"

Object.prototype.toString.call(new RegExp()); //"[object RegExp]"

Object.prototype.toString.call(Symbol()); //"[object Symbol]"

Object.prototype.toString.call({}); //"[object Object]"

Object.prototype.toString.call([]); //"[object Array]"

Object.prototype.toString.call(null); //"[object Null]"

Object.prototype.toString.call(undefined); //"[object Undefined]"
```





