# canvas
```html
<canvas id="canvas" width="500" height="500">
<script>
    var canvas = document.getElementById('canvas');
    var context = canvas.getContext('2d');
    //进行context操作
</script>
```
# canvas坐标系
* 左上角为原点

# 绘制矩形
## context.fillRect(x,y,宽，高) 实心矩形
## context.strokeRect(x,y,宽，高) 空心矩形

# 绘制线条（路径）
* context.beginPath();//开始绘制
* context.moveTo();//起点
* context.lineTo();//画向的点
# 绘制多边形
* context.closePath();
* context.stroke();`不写这句没有路径`
* context.fill();
# 绘制弧形
* context.arc(x,y,radius,startAngle,endAngle,false);
    * x，y:圆心
    * radius: 半径
    * startAngle，endAngle: 2*Math.PI为360度，最右为0PI(2PI)
    * 最后一个参数为顺时针还是逆时针
# 绘制文本
* context.font = '60px 黑体';`必需两个参数`
* context.fillText(text,x,y)
    * text:文本
    * x,y为文本左下角
* context.strokeText(text,x,y)
# 上色
* context.fillStyle = color;
* context.strokeStyle = color;
* 应用于设置的代码后面的操作
* color需符合css3颜色值标准
    * red
    * #ff0000
    * rgb()
    * rgba()
# 线宽
* context.lineWidth = number;
# 擦除canvas
* clearRect(x,y,width,height)
    * x,y:左上角
    * width和height：可以用canvas.width和canvas.height
* 清除所选矩形区域
# 加载图像
* context.drawImage(image,x,y,[width],[height])
    * 中括号表示可选
    * 图片的宽和高
    ```javascript
    var image = new Image();
    image.src = "https://";
    image.onload = function() {
        context.drawImage(image,50,100);
    }
    ```
# 裁剪图像
* context.drawImage(image,s_x,s_y,s_width,s_height,x,y,width,height);
    * image:源图像对象
    * s_x，s_y：裁剪原点
    * s_width,s_height:裁剪的宽度和高度
    * x,y,width,height:裁剪后绘制的图像属性6

