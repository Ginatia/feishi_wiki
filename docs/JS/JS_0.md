
## 变量

```js
// 可变
let x = 1;
let msg = "Hello";

// 不可变
const y = 2;
```

## 循环

```js
let x = 0

while(x<10){
    console.log(x);
    x++;
}

let y = 0;
do{
    console.log(y);
    y++;
}while(y<10);

for(let i=0;i<10;i++){
    console.log(i);
}
```

## 函数

```js
function addition(num) {
    return num + 1;
}
```

### 函数是对象

直接利用此特性，实现函数回调。

```js
function showOn(){
    console.log("ON");
}

function showOff(){
    console.log("Off");
}

function show(state,showOn,showOff){
    if(state == true){
        showOn();
    }
    else{
        showOff();
    }
}

show(true,showOn,showOff);

show(false,showOn,showOff);
```

### 箭头函数

```js
let f = (x,y) => {
    return x + y;
}

console.log(f(1,2))
```

## Object

```js
let o = {
    name: "feishi",
    age: 22
};

for(let key in o){
    console.log(o[key]);
}
```

### 对象方法

```js
let o = {
    name: "feishi",
    age: 22,

    hello: function(){
        console.log(this.name,this.age);
    }
};

o.hello();
```

### 构造对象函数

```js
function User(name,age){
    this.name = name;
    this.age = age;

    this.hello = function(){
        console.log(this.name,this.age);
    };
}

let u1 = new User("feishi",22);
let u2 = new User("ginatia",18);

u1.hello();
u2.hello();
```

## 类

```js
class User{
    constructor(name,age){
        this.name = name;
        this.age = age;
    }
    
    hello(){
        console.log(this.name,this.age);
    }
}

let u = new User("feishi",22);

u.hello();
```



