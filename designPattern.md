# 设计模式
* 模式是已经经过验证的解决方案，容易被复用，且富有表达力

## 反模式
* 反模式是一种针对某个特定问题的不良解决方案，该方案会导致糟糕的情况发生
1. 定义大量污染全局命令空间的变量
2. 修改Object类的原型
3. 使用document.write创建页面
4. 硬编码，写死功能
5. 向setTimeout()传入字符串，会导致内部触发 eval() 的使用。eval() 方法会造成性能问题和可能会引起安全性的问题。
6. ...

## 单例模式（singleton pattern）
* 确保一个类只有一个实例共享使用
* eg: 皇帝、登录窗口、计时器、应用程序中配置对象（一份配置一个实例对应）等
    ```javascript
    var kingSingleton = (function() {
        var instance;
        function init() {
            return {
                command: function(){}
            }
        }
        return {
            getInstance() {
                if(!instance) {
                    instance = init();
                }
                return instance;
            }
        }
    })()
    ```
* 优点：节约资源，控制入口
* 缺点：拓展困难，职责不可过重，因为都在一个实例上实现

## 适配器模式（adapter pattern）
* 不影响现有的实现方式，兼容调用旧接口的代码（适配操作中使得兼容旧接口代码，eg: 数组转换成对象）
* `优点`： 简单、提高复用性
* `缺点`： 过多使用导致系统层级复杂

## 装饰器模式（decorator pattern）
* 在不修改原来的接口下，动态地为对象添加功能
```javascript
function BeanMilk() {
    this.price = 1;
}
var beanMilk = new BeanMilk();
function addEgg(milk) {
    milk.price += 0.5;
}
```
* `优点`：扩展对象更灵活可组合
* `缺点`: 更复杂

## 观察者模式（发布者订阅者模式）
```javascript
//观察者
function Observer() {
    this.update = function() {
    }
}
function Subject() {
    this.observers = [];
}
var xiaoxin = new Observer('小新');
var daxin = new Observer('大新');
Subject.prototype.addObserver = function(observer) {
    this.observers.push(observer);
}
Subject.prototype.removeObserver = function(observer) {
    this.index = this.observers.indexOf(observer);
    this.observers.splice(index,1);
}
Subject.prototype.update = function() {
    this.observers.map(obs=>{
        obs.update(context);
    })
}
var xiaod = new Subject('小D');
```

## MVC模式
* 关注点分离改进组织程序
* view：视图，观察业务数据（Model模型），调用应用逻辑（Controller控制器）
* model：模型，事件通知用户界面（view视图）
* controller：控制器，修改业务数据（Model模型）
* 简单实现MVC模式
```html
<html>
<div id="container">
    <div id="num"></div>
    <div id="add-btn"></div>
    <div id="sub-btn"></div>
</div>

</html>
<script>
function View(controller) {
    var container = document.getElementById('container');
    var addBtn = document.getElementById('add-btn');
    var subBtn = document.getElementById('sub-btn');
    var num = document.getElementById('num');

    this.render = function(model) {
        num.innerHTML = model.getVal() + '元';
    }

    addBtn.onclick = controller.increase;
    subBtn.onclick = controller.decrease;
}
function Model(view) {
    var num = 0;
    var views = [];
    function sub() {
        if (this.num>0) {
            this.num--;
        }
    }
    function add() {
        if(this.num < 100) {
            this.num++;
        }
    }
    this.getVal = function() {
        return num;
    }
    //观察者模式
    var self = this;
    this.notify = function() {
        views.map(view => view.render(self));
    }

    this.register = function(view) {
        views.push(view);
    }
}
function Controller() { 
    var view = null,model = null;
    this.init = function() {
        var view = new View();
        var model = new Model();
        model.registry(view);
        model.notify();
    }
    this.decrease = function() {
        model.sub();
        model.notify();
    }
    this.increase = function() {
        model.add();
        model.notify();
    }    
}
var controller = new Controller();
controller.init();
</script>
```

### 弊端
* 浏览器已实现独立处理事件
* 大量事件则需要大量修改model和controller

## MVVM
* 把`MVC`的controller改成viewModel，model模型数据修改时视图更新，视图修改时，model模型自动更新
![MVVM.png](./images/mvvm.png)
* `优点`:view视图和model模型双向绑定，自动化同步
* 不适用于简单UI界面

### MVVM实现原理
* Object.defineProperty(obj,key,value)
* value可传配置{
    value: someValue,
    writable：true,//可写
    enumerable：true,//可被赋值
    configurable：true//可改变和删除
}
* 数据劫持实现
```javascript
function defineProperty() {
    Object.defineProperty(obj,attr,{
        get: function() {
            return value;
            //实现绑定操作
        },
        set: function(val) {
            value = val;
            //实现更新操作(发布更新操作)
        }
    })
}

```

## jquery的设计模式
* 迭代器模式
提供一种方式遍历集合所有的元素，且又不暴露该对象的内部实现
```javascript
$.each(arr,function(i,value){})
$("span").each(function(index) {})
```
* 观察者模式
```javascript
//等价于订阅和观察者更新操作update
$(document).on('topicName',function(){});
//等价于通知操作notify
$(document).trigger('topicName');
//等价于取消订阅unsbscribe(主题名称)
$(document).off('topicName');
```
* 组合模式
描述了一组对象可像单个对象一样地对待
```javascript
$('#singleSpan').addClass("disabled");
$('span').addClass("disabled");
```