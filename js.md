# 3.js作用域
···javascript
1) function级 或 let/const(块级作用域)
2) let--es6, let声明的变量不允许重复声明 let bar= foo*2;
3) const-es6, 块级常量不可修改 const b = 4;
4) 作用域链-逐级向上查找(直到顶部window,未找到报错)，scope chain
```
****

# Js关键字
**this**
```javascript
//既不指向自身，也不指向自身作用域，指向调用它的那一层作用域(调用位置，不是声明位置)
function foo(){
	var a=2;
	this.bar();
}
function bar(){
	console.log(this.a);
}
foo();	//undefined
````
