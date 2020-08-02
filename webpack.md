# webpack
* 理解前端模块化
* 理解webpack打包的核心思路
* 理解webpack中的“关键任务”：loader和plugin

## 1. 模块化
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
* 优点且是目的
    1. 作用域封装
    2. 重用性
    3. 接触耦合（保持API不变）

## 2. webpack打包机制
1. 从入口文件开始，分析整个应用的依赖树
2. 将每个依赖模块包装起来，放到一个数组中等待调用
3. 实现模块加载的方法，并把他放到模块执行的环境中，确保模块间可以互相调用
4. 把执行入口的文件的逻辑放到一个函数表达式中，并立即执行这个函数

## 3. webpack的用处
1. package.json中的script标签，

### 3.1 dependencies & devDependencies
* npm install --only=dev
* npm install --only=prod
* dev:
    1. 构建工具
    2. 校验工具

### 3.2 语义化版本
* ^version:^1.0.1 中版本，小版本1.x.x
* ~version:~1.0.1 小版本 1.0.x 
* version:特定版本

## 4. npm install过程
1. 寻找包版本信息文件（package.json）,依照它来安装
2. 查package.json中的依赖，并检查项目中其他的版本信息文件
3. 如果发现了新包，就更新版本信息文件