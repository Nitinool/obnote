引入方式
	内嵌式 head里面写入script标签
	外部引入 <script/ src=".js" type="">

数据类型（弱类型）
	js变量声明统统使用var 赋值时才有具体类型
	





js如何声明函数？  function fuc_name(){}
```javascript
function func_name(){
	alert("hello")

}
```

js如何和用户行为绑定到一起？  定义一个行为属性，值为js函数
```html
<button class="btn1" onclick=" func_name()">  </button>
``` 

如何弹窗提示?  在js函数里面使用alert()