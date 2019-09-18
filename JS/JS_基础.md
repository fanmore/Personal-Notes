## 数据类型
* String: 字符类型
* Number: 数值类型
* Boolean: 布尔类型 true/false
* Undefined: 未定义 定义变量未赋值
* Null: 空值
* Object: 对象类型

说明：Object是复杂类型，前面几个是基础类型，在ES6中多一个类型Symbol，独一无二的类型。

## Object
获取和访问方式：
```js
var goods = {
	name: "Mac"
    price: 8000
};

console.log(goods.name);
console.log(goods["name"]);
//结果一样，只是上面种写法固定，而下面种写法支持变量替换

var s = "name";
console.log(goods[s]);
```
