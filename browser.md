# 浏览器渲染引擎
* Chrome(Blink)
* Edge/IE(Trident)
* Safari(Webkit)
* firefox(Gecko)
* Opera(Blink)

# HTML到Dom树
* 词法解释（最小单元）
* 语法解释（语法规则->扫描->形成DOM树）

# 渲染过程
* 边加载边解析边渲染
* DOM Tree和Render Tree
    * 一一对应
    * DOM Tree中display：none
    * position：absolute/fixed的Render Tree中只是个占位符，脱离常规渲染
    
# 重排与重绘
## 重排
* 页面第一次渲染
* 浏览器窗口尺寸改变
* 元素位置和尺寸
* 新增和删除可见元素
* 内容改变
## 重绘
* color
* background-color