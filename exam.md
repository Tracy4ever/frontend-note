1. setInterval和setTimeout区别
* setTimeout和setInterval都有延时问题
* 是event loop中的宏任务
* 都是把事件放入event loop，放进event queue后等待主线程完成后再调用event queue里面的函数
* 区别是setInterval，如果第一个回调超过定时时间，第二个回调在event queue上，则第三个回调第四个回调即使到了执行时间也被遗弃，且会在定时时间的可执行最小倍数内执行，则无法保证回调执行间隔时间。
* setTimeout实现setInterval则可以保证间隔时间。
* [参考文章](http://qingbob.com/difference-between-settimeout-setinterval/)

2. if和for
* if和for是块级作用域，var不支持块级作用域则会提升到函数作用域或者全局作用域

3. 判断NaN
* NaN===NaN;//false
* Object.is(NaN,NaN)//true
* Number.isNaN(NaN)//true
* 判断0与-0
	* 0 === -0; //true
	* Object.is(0,-0); // false
	* function isNegativeZero(n) {
		return 0 === n && 1/n === - Infinity;
	}

4. undefined和Null
[参考文章](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)
[参考文章](https://juejin.im/post/5a9f92eb6fb9a028c522bbf1)
* 相同：
  1. 在if语句中转换成false
  2. parseInt(),parseFloat()均返回NaN
* 不同：
  1. null被定义，定义为空值；undefined表示根本未定义
  2. null转为数值为0；undefined转为数值为NaN
  3. JSON序列化：JSON规范中被强化，这个标准中不存在 undefined 这个类型，但存在表示空值的 null 。在一些使用广泛的库（比如jQuery）中的深度拷贝函数会忽略 undefined 而不会忽略 null ，http接口的报文参数也是如此。
  4. typeof null;//object 
     typeof undefined;//undefined
  5. 设置为null的变量或者对象会被内存收集器回收，undefined不会。


5. cookie和session和sessionStorage和localStorage
[参考文章](https://juejin.im/post/5d82e46a51882556ba55e6a5)
* 时效性：
    1. cookie不设置expires、max-age则关闭浏览器时失效；
    2. web.xml配置，通过cookie传输sessionId，cookie失效则session失效（cookie不设置超时，则关闭浏览器session也失效）
    3. sessionStorage在页面会话期可用（浏览器打开有效，页面重新加载和恢复也有效）
    4. localStorage浏览器关闭，重新打开有效
* 容量：
    1. 每个cookie长度不超过4KB
    2. web storage存储量5MB
* 保存地方：
    1. cookie保存在客户端
    2. session保存在服务器，只通过cookie发送sessionId
    3. web storage存储在本地，浏览器会话中
* 场景：
    1. cookie存储用户信息（token），标记用户行为（埋点），客户端和浏览器信息传递
    2. session是会话，客户端与服务器一系列交互的动作称为一个Session
    3. web storage用于本地大容量存储数据

# 6. 性能优化
## 6.1 压缩体积，合并资源，减少HTTP请求
## 6.2 [DNS预解析](https://www.cnblogs.com/rongfengliang/p/5601770.html)
## 6.3 使用CDN
## 6.4 使用缓存
## 6.5 非核心代码异步加载
* <script async src=""></script>
  * async脚本加载完就执行，在load事件之前执行，但无法判断在DOMContentLoad的前后
* <script defer src=""></script>
  * defer在HTML解析完后执行，如果是多个则按照加载顺序依次执行；defer脚本会在DOMContentLoaded和load事件之前执行


7. 节流防抖
```javascript
//节流
function throttle(func,time) {
  var timeout;
  return function() {
    if(!timeout) {
      timeout = setTimeout(() => {
        timeout = null;
        func.apply(this,arguments);
      },time)
    }
    
  }
  
}
//防抖
function debounce(func,time) {
  var timeout;
  return function() {
    if (timeout) {
      clearTimeout(timeout);
    }
    timeout = setTimeout(()=>{
      func.apply(this,arguments);
    },time)
  }
  
}
```

# 8. 垂直居中
## 8.1 absolute和margin
* 需要知道inner（子元素）的宽高
## 8.2 absolute和calc
* 需要知道inner的宽高
## 8.3 absolute和auto margin
* 需要知道inner的宽高
```css
.inner {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
```
## 8.4 absolute和transform
* 需要依赖translate的兼容性
* transform:translate(-50%,-50%)
## 8.5 table
```css
.out {
    border: 1px solid #000;
    display: table-cell;
    width: 300px;
    height: 300px;
    text-align: center;
    vertical-align: middle;
}
.inner {
    border: 1px solid #333;
    display: inline-block;
}
```
## 8.6 flex
```css
.out {
  display: flex;
  align-items： center;
  justify-content: center;
}
```
## 8.7 grid
### 8.7.1 父元素指定子元素对齐
```css
.out {
  display: grid;
  align-items: center;
  justify-content: center;
}
```
### 8.7.2 子元素指定父元素对齐
```css
.out {
  display: grid;
}
.inner {
  align-self: center;
  justify-self: center;
}
```

# 9. for in 和for of
* for in更适合遍历对象
* for in遍历的是数组的`索引（即键名）`，而for of遍历的是数组`元素值`。
* for of遍历的只是数组内的元素，而`不包括数组的原型属性method和索引name`
* for in 可以遍历到myObject的原型方法method,如果不想遍历原型方法和属性的话，可以在循环内部判断一下,hasOwnPropery方法可以判断某属性是否是该对象的实例属性
* 遍历map对象适合用解构
```javascript
for(var [key,value] of phoneBookMap){}
```

# 10. 字符串和操作符
* 字符串执行+操作则字符串拼接
* 字符串执行-操作则数值计算

# 11. css设置不能点击
* 不能点击图标
cursor：not-allowed
* 显示点击图标，但不能触发点击事件
pointer-events： none

# 12.特殊
* typeof null//object
* typeof function//function
* typeof []//object
* NaN === NaN //false

# 13. 深拷贝 & 浅拷贝
## 浅拷贝
1. 场景：Object.assign
## 深拷贝
1. JSON.parse(JSON.stringify())
* 忽略一些值
  1. undefined
  2. symbol 
  3. 循环引用的对象
  4. function函数
  5. new Date()
  6. 正则
