# -
宽松等于的情况下，有布尔值的情况需要注意的地方

==在js中一般都不会使用，一般情况下我们会使用`===`严格相等。今天看`《你不知道的js》--中卷--第四章`的时候，发现书中讲解了关于==的情况。特别是在一方为布尔类型的情况下特别有意思。

先看下放例子：
```javascript
let str = '42';
console.log(str == true);  // false
console.log(str == false); // false
```
如果我们从布尔类型的角度看，可能会理解为变量str既不是真值也不是假值。其实不然。

js规范中这样规定布尔值比较的：
>  如果 Type(x)是布尔值，则返回ToNumer(x) ==  y的结果
>  如果 Type(y)是布尔值，则返回 x == ToNumber(y) 的结果。
*用一句话说就是，如果存在布尔值的情况下，会首先将布尔值转为对应的number类型，在进行比较。如果比较的类型时number类型，会直接进行比较，如果是其他类型，则会发生强制类型转换。*

根据规定，其实上面例子中的情况是这样子的：
```javascript
let str = '42';
console.log(str == Number(true));
console.log(str == Number(false));
```
首先会把布尔值true或者false转为number类型，值为1或者0。接下来会发生强制类型转换，比较 42 == 1 或者 42 == 0。这两种情况都是返回false的。

**所以我们在判断条件中，不要出现以下结果**
```javascript
let xx = '66';
if(xx == true){
  console.log('不会进入该判断条件内');
}
if(xx == false){
  console.log('不会进入该判断条件内');
}

// 最好写成如下形式
if(!!xx){
  console.log('会进入该判断条件内');
}
if(Boolean(xx)){
  console.log('也会进入该判断条件内');
}
```

接下来在看看下面的情况：
```javascript
console.log("0" == false); // true
console.log(0 == false); // true
console.log("" == false); // true
console.log([] == false); // true
console.log({} == false);; // false
```
根据规定，前三种我们都知道什么结果

主要看一下对象和数组的情况下的结果。

js的基本类型有包装对象，这点都比较清楚，和包装对象相对应的是拆封。

```javascript
[] == false;
```
上面代码的操作过程，其实是先让[]执行valueOf以及toString()方法，在和false进行比较。
```javascript
console.log([].valueOf(),[].toString()); // [], '''
// 相当于是 '' == false 的比较
```
```javascript
console.log({}.valueOf(), {}.toString()); // '[object Objecet]'
// 相当于是 '[object Objecet]' == false 的比较。所以返回false
```
