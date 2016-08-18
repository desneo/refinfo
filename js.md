
# 2.js基础
```javascript
<script src="/servletexample/resources/js/jquery-3.1.0.js"></script>
<script src="http://cdn.staticfile.org/jquery/2.1.1-rc2/jquery.js"></script>
```

# 3. js作用域
```javascript
1) function级 或 let/const(块级作用域)
2) let--es6, let声明的变量不允许重复声明 let bar= foo*2;
3) const-es6, 块级常量不可修改 const b = 4;
4) 作用域链-逐级向上查找(直到顶部window,未找到报错)，scope chain
```
****

# 50. Js关键字
**this**
```javascript
//既不指向自身，也不指向自身作用域，指向调用它的那一层作用域(调用位置，不是声明位置)
function foo(){
	var a=2;
	this.bar();
}
function bar(){ console.log(this.a); }
foo();	//undefined
````

# 60. JQuery
## 60.1基础
```javascript
//绑定动作
<input id="upload" type="button" value="上传" />
$("#upload").click(function(){ //ToDo }）
```

## 60.20 jquery上传文件
```javascript
//使用 FormData 对象添加字段方式, IE11支持
<input id="upload" type="button" value="上传" />
<script>
	$("#upload").click(function(){
		debugger;
		var formData = new FormData();
		formData.append('file', $('#file')[0].files[0]);
		$.ajax({
			url : '/servletexample/upload',
			type : 'POST',
			cache : false,
			data : formData,
			processData : false,
			contentType : false
		}).done(function(res) {
			alert("上传成功");
		}).fail(function(res) {
		});
	});
</script>
```

# 70. AngularJs
**调试**  
```
//chrome	console中找到当前页frame --> $($0).scope().$Gadget.queryPhoneNumber
//IE 	$(document).find('iframe')[5].contentWindow.$("#inputText_PhoneNumber").scope()
```
**指令**  
```javascript
//ng-show ng-hide	ng-bind 	ng-disabled 	ng-readonly
//ng-class
	1) ng-class="{positive: $index==1 && values > 0, negative:values < 0 && $index==1 }
	2) ng-class="{true: 'active', false: 'inactive'}[isActive]"	//isActive==true时添加active
//ng-href ng-src  当<img><a>绑定数据时，由于浏览器并行方式加载图片和其他内容，故angularjs没有机会拦截到数据绑定的请求!
	<img ng-src="/images/{{favoriteCat}}">
	<a ng-href="/shop/category={{numberOf}}"> some txt </a>

```

**事件**  
```javascript
//ng-click
<input ng-change="computeNeeded()" ng-model="funding.startingEstimate" >
//ng-keydown	ng-keyup	ng-change	ng-mousedown	ng-mouseenter	ng-mouseleave
```
