# requestAnimationFrame
* 动画循环最佳间隔16.7ms，60Hz，每秒渲染60次画面
```javascript
// 判断是否有 requestAnimationFrame 方法，如果有则模拟实现
window.requestAnimFrame =
window.requestAnimationFrame ||
window.webkitRequestAnimationFrame ||
window.mozRequestAnimationFrame ||
window.oRequestAnimationFrame ||
window.msRequestAnimationFrame ||
function(callback) {
    window.setTimeout(callback, 1000 / 30);
};
```
* 实现动画在animation.html

# 动画循环
* 1. 初始化数据
* 2. 动画循环（更新，清除，绘制）
* 3. 结束动画循环（边界条件）

# 键盘事件
* `keydown`：当用户按下键盘上的任意键时触发，而且如果按住按住不放的话，会重复触发此事件。
* `keypress`：当用户按下键盘上的字符键时触发，而且如果按住不放的，会重复触发此事件（按下Esc键也会触发这个事件）。
* `keyup`：当用户释放键盘上的键时触发。
* 用户按下键盘上的字符键时,首先会触发`keydown`事件,然后紧接着触发`keypress`事件,最后触发`keyup`事件,如果用户按下了一个字符键不放，就会重复触发`keydown`和 `keypress` 事件，直到用户松开该键为止。
# 键码
[键码参考](https://wenku.baidu.com/view/983e918ace84b9d528ea81c758f5f61fb73628b6.html)
```javascript
//兼容写法
// 全局监听键盘操作的 keydown 事件 
      document.onkeydown = function(e) {  
        // 获取被按下的键值 (兼容写法)
        var key = e.keyCode || e.which || e.charCode;
      }
```l