# 模块化
# 1. 命名空间
* JavaScript 语言本身并没有命名空间这一个概念，为了避免变量冲突，需要通过各种手段来模拟实现命名空间的功能。
    * `前缀命名空间`
        * `优点`：有效防止命名冲突
        * `缺点`：仍然定义了大量全局变量
    * `对象命名空间/嵌套对象命名空间`
        var moduleB = {
            b: 0,
            methodB: function(){}
        }
        * `缺点`：所有属性和方法都会暴露，没有私有变量
    
    * `IIFE立即执行函数表达式`(Immediately Invoked Function Expression)
        ```javascript
        //立即执行的函数且只执行一次
        function immediate() {
        }
        immediate();

        //IIFE两种方式
        (function iife(){})()
        (function iife(){}())

        //IIFE实现模块
        var moduleB = (function(){
            var a = 1;
            var b = 0;
            function methodB (){};
            function keep(){}
            return {
                b: b
                keep: keep
            }
        })()
        ```
        * `标准实现`
        ```javascript
            (function(window) {
                var name = 'Susan';
                var sex = '女孩';
                function tell() {
                    console.log('我的名字',name);
                    console.log('我的性别',sex);
                }
                window.Susan = {tell}
            })(window)
        ```
        * `优点`： 
            1. 创建私有作用域，避免变量冲突
            2. 使用IIFE实现模块，`包裹模块代码，暴露模块属性和接口，封装私有变量`
        * `拓展`：为什么不能直接在函数后面加括号表示直接执行？
            * JavaScript引擎规定，如果function关键字出现在行首，一律解释成语句。因此，JavaScript引擎看到行首是function关键字之后，认为这一段都是函数的定义，不应该以圆括号结尾，所以就报错了。


# 2. 模块化规范
* `目的`: 统一方法定义模块，不需要手动维护依赖 
* `三种规范`： 
    1. commonJS
        * 文件即模块
        * 使用module.exports(exports)暴露对外的接口（对象的形式）
        * 使用require同步加载依赖模块
    * commonJS`同步加载`模块，node服务器的文件存在`本地硬盘`，加载快且可同步加载
    * 浏览器端文件需要`网络加载`，耗时且同步加载会`阻塞页面`，所以浏览器不适用commonJS,需要`异步加载模块的规范`

        ```javascript
        //math.js
        function area(r) {
            return Math.PI * r * r;
        }
        module.exports = {
            area: area
        }
        //另外一种暴露接口的方法
        //exports.area = function(r) {}
        //circle.js
        var math = require(./math.js);
        var radius = 10;
        math.area(radius);
        ```
    2. AMD异步模块定义（Asynchronous Module Definition）
        * 原生JS不支持AMD，所以需要模块加载器，requireJS是AMD的模块加载器
        * 使用define函数定义模块
            define(id?,dependecies?,factory)
            * `id`: 模块标识，不传则是加载的脚本文件的名字
            * `dependencies`: 依赖的模块数组，默认(不输入则为)['require','exports','module']
            * `factory`: 模块初始化要执行的函数或者对象，对象则当前模块的导出值，函数则return出去的是模块的接口、导出值
            ```javascript
            //moduleA.js
                define('moduleA',function(require,exports,module) {
                    exports.getNum = function(x) {
                        return 5;
                    }
                })
            //moduleD.js
                define('moduleD',['moduleA'],function(moduleA) {
                    var index = moduleA.getNum();
                    return {
                        addIndex: function() {
                            index++:
                        }
                    }
                })
            ```
        * 使用require函数调用模块
            require(modules,callback);
            * `modules`: 加载的模块Id的数组
            * `callback`: 加载完模块的回调
            ```javascript
            require(['moduleA','moduleB'],function(moduleA,moduleB) {
            })
            ```
    3. CMD通用模块定义（Common Module Definition）
        * SeaJS是CMD的模块加载器即具体实现
    4. CMD和AMD的不同
        1. 声明依赖模块的方式不同
            * CMD推崇`依赖就近`：用到某个模块的时候再去require
            * AMD推崇`依赖前置`：定义模块的时候就要声明其依赖模块
            ```javascript
            //CMD
            //<script src="./seaJs">
            //javascript文件中
            define(function(require, exports, module) {
                var c = 10;
                var a = require('./a')
                a.doSomething()
                // 依赖可以就近书写
                var b = require('./b') 
                b.doSomething()
                exports.c = {
                    c: c
                }
            })

            // AMD 默认推荐的是 依赖必须一开始就写好
            define(['./a', './b'], function(a, b) { 
                a.doSomething()
                b.doSomething()
            }) 
            ```
        2. 执行依赖模块时机
            * AMD `提前执行依赖`（`异步加载`：依赖先执行）+延迟执行
            * CMD `延迟执行依赖`（运行到需加载，根据`顺序执行`）
        3. 加载器
            * AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。
            * CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。

# 3. ES6模块标准
* 从语言层面去解决模块标准规范的问题，实现简单(import/export)
* 浏览器和服务器通用的模块解决方案

## 3.1 ES6模块的基本特点
* 自动开启`严格模式`，即使没有写use strict
* 每个模块有自己的`上下文`，每一个模块内声明的变量都是`局部变量`,不会污染全局作用域
* 模块中可以导入和导出各种类型变量
* 每个模块只加载一次，每个JS只执行一次，下次加载从缓存中获取，一个模块就是一个单例，或者一个对象

## 3.2 export
1. `常量`: export const a = 1;
2. `函数`: export function calc() {}

## 3.3 import
import {calc} from './a';

### 拓展
[import和require的区别](https://www.cnblogs.com/suneil/p/11289884.html)

## 4. 工程化的模块化管理
* webpack(打包压缩工具) 和 mod.js(模块加载器)
