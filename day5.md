# Today's schedule

## 熟悉ES 6



```javascript
//Expression bodies 
var odds  = evens.map(v => v+1)
var nums = evens.map((v,i) => v+i)
//Statement bodies
nums.forEach( v =>{
    if(v % 5 ===0)
        fives.push(v)
})
// Lexical this
var bob = {
    _name:"Bob",
    _friends:[],
    printFriends(){
        this._friends.forEach(
        f => console.log(this._name+"Knows"+f))
    }
}
// Lexical arguments
function square(){
let example = () =>{
    let numbers = [];
    for (let number of arguments){
        numbers.push(number*number)
    }
    return numbers
}
return example()
}
square(2,4,7.8,8,11.4,21)

```

class

```javascript
class SkinnedMesh extends THREE.Mesh{
    constructor(geometry,materials){
        super(geometry,materials);
        
        this.idMatrix = SkinnedMesh.defaultMatrix()
        this.bones = []
        this.boneMatrices = []
    }
    update(camera){
        super.update()
    }
    static defaultMatrix(){
        return new THREE.Matrix4()
    }
}
```

增强对象文字

```javascript
var obj = {
    __proto__:theProtoObj,
    ['__proto__']:somethingElse,
    handler,
    toString(){
        return "d"+super.toString()
        
    }
    ["prop_"]+(() => 42)()]:42
}
```

template string

```javascript
var name = "Bob",time = "today"
Hello ${name},how are yo ${today}
```

解构

```javascript
var [a,,b] = [1,2,3]
a ===1;//true
b ===3//false


function g({name:x}){
    console.log(x)
}
g({name:5}) // x只能为数字


var [a] = [];
a === undefined;// true

var [a =1 ] = [];
a === undefined;//false
a===1 //true

function r(x,y,w =10,h =10){
    return x+y+w+h
}
r({x:1,y:2}) //23
```

默认+休息+传播

```javascript
function f(x,y =12){
    return x+y;
}
f(3) ==15 //true 

****
function f(x,...y){
    // y is an Array
    return x* y .length
}
f(3,"hello",true)==6 //true
f(3,"hello",true,"sdsad","dasdas") ==12 //true

function f(x,y,z){
    return x+y+z
}
f(...[1,2,3]) ==6 //true
f(1,2,3) == 6 //true



```

let + const

```javascript
function f (){
    {
        let x ;
        
        {
            // ok
            const x = "sneaky"
            //error 刚刚用const定义
            x = "foo"
        }
        // OK
        x = "bar"
        //上面已经在此块声明雷
        let x = "inner"
    }
}
```

迭代器+ for ..of

```javascript
let fibonacci ={
    [Symbol.iterator](){
        let pre = 0 ,cur = 1 ;
        return{
            next(){
                [pre,cur] = [cur,pre+cur]
                return {done:false,value:cur}
            }
        }
    }
}


for (var n of fibonacci){
    if(n>1000)
        break
    console.log(n)
}
```

生成器

```javascript
var fibonacci = {
    [Symbol.iterator]:function*(){
        var pre = 0,cur =1 
        for (;;){
            var temp = pre
            pre = cur 
            cur + = temp 
            yield cur
        }
    }
}
for (var n of fibonacci) {
    if (n>1000)
        break
    console.log(n)
}

```

生成器接口:

```javascript
interface Generator extends Iterator{
    next(value?:any):IteratirResult
         throw(exception :any)
}
```

Unicode

```javascript
// same as ES5.1
"𠮷".length == 2

// new RegExp behaviour, opt-in ‘u’
"𠮷".match(/./u)[0].length == 2

// new form
"\u{20BB7}" == "𠮷"
"𠮷" == "\uD842\uDFB7"

// new String ops
"𠮷".codePointAt(0) == 0x20BB7

// for-of iterates code points
for(var c of "𠮷") {
  console.log(c);
}
```

模组

对组件定义模块的语言级别支持。整理来自流行的JavaScript模块加载器（AMD，CommonJS）的模式。由主机定义的默认加载器定义的运行时行为。隐式异步模型–在可用和处理所请求的模块之前，不会执行任何代码

Map + Set + WeakMap + WeakSet

```javascript
// Sets
var s = new Set()
a.add("hello").add("goodbye").add("hello")//{"hello","goodbye"} 集合 无重复
s.size ===2 //true
s.has("hello") === true// true


// Maps
var m = new Map();
m.set("hello",42)
m.set(s,34)  // {"hello" => 42. Set(2) => 34}
m.get(s) == 34 //true

// Weak Maps
var wm = new WeakMap()
wm.set(s,{extra:42})
wm.size === undefined
```

`WeakSet` 对象是一些对象值的集合, 并且其中的每个对象值都只能出现一次.

它和 set对象的区别有两点:

- `WeakSet` 对象中只能存放对象引用, 不能存放值, 而 `Set` 对象都可以.
- `WeakSet` 对象中存储的对象值都是被弱引用的, 如果没有其他的变量或属性引用这个对象值, 则这个对象值会被当成垃圾回收掉. 正因为这样, `WeakSet` 对象是无法被枚举的, 没有办法拿到它包含的所有元素.
- 

**`WeakMap`** 对象是一组键/值对的集合，其中的键是弱引用的。其键必须是对象，而值可以是任意的。

```js
var wm1 = new WeakMap(),
    wm2 = new WeakMap(),
    wm3 = new WeakMap();
var o1 = {},
    o2 = function(){},
    o3 = window;

wm1.set(o1, 37);
wm1.set(o2, "azerty");
wm2.set(o1, o2); // value可以是任意值,包括一个对象
wm2.set(o3, undefined);
wm2.set(wm1, wm2); // 键和值可以是任意对象,甚至另外一个WeakMap对象
wm1.get(o2); // "azerty"
wm2.get(o2); // undefined,wm2中没有o2这个键
wm2.get(o3); // undefined,值就是undefined

wm1.has(o2); // true
wm2.has(o2); // false
wm2.has(o3); // true (即使值是undefined)

wm3.set(o1, 37);
wm3.get(o1); // 37
wm3.clear();//没有这个方法
wm3.get(o1); // undefined,wm3已被清空
wm1.has(o1);   // true
wm1.delete(o1);
wm1.has(o1);   // false
```

代理

代理使对象的创建具有可用于宿主对象的全部行为.可以用于拦截,对象虚拟化,日志记录/分析等

```js
var target = {}
var handler = {
    get:function(receiver,name){
        return 'hello,$(name)!'
    }
}

var p = new Proxy(target,handler)
p.world === "Hello,world!"
```

符号

符号启用对对象状态的访问控制.使用符号可以通过string键或键来键入属性symbol,符号是一种新的原始类型,name 调试中使用的可选参数,但不是标识的一部分.符号是唯一的 但不是私有的,因为他们通过反射特征暴露出来

```js
(function() {

  // module scoped symbol
  var key = Symbol("key");

  function MyClass(privateData) {
    this[key] = privateData;
  }

  MyClass.prototype = {
    doStuff: function() {
      ... this[key] ...
    }
  };

  // Limited support from Babel, full support requires native implementation.
  typeof key === "symbol"
})();

var c = new MyClass("hello")
c["key"] === undefined
```

