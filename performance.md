#  性能
* 性能 -> 影响用户体验 -> 影响网站收益

# 1. window.performance 高精度测量网站性能
* [API](https://javascript.ruanyifeng.com/bom/performance.html#toc4)

# 2. 性能衡量指标
* `开始请求`：window.performance.navigationStart
* `获取首字节`： window.performance.responseStart（衡量指标）
* `页面开始展示`（白屏时间）:衡量指标
* `首屏内容加载完成`：衡量指标
* `加载完成`

# 3. 计算白屏时间
白屏时间 = 页面开始展示的时间点 - 开始请求的时间点
* `开始请求的时间点`：window.performance.navigationStart
    * 如果浏览器不兼容window.performance，则在head标签里，`meta/title标签后`加script标签，获取开始请求的时间点
* `页面开始展示的时间点`：head标签的末尾
    * 文档流从上到下解析，先解析head，再解析body，解析body开始看到内容
    * 在`head标签末尾`加script标签，执行到new Date()获得的时间

# 4. 计算首屏时间
首屏时间 = 首屏展示完成时间 - 开始请求的时间
* `首屏展示完成时间`：人工判断首屏展示的范围，在该标签下插入script标签获取执行到该标签下的时间
## 4.1 图片懒加载

# 5. 优化手段
* `减少次数`：减少请求的文件数（提高加载速度，减少服务端压力）
    1. 小图片使用雪碧图或者使用iconfont
    2. 合并JS文件和CSS文件
* `减少体积`：减少资源体积，压缩等
    1. 精简代码
    2. 压缩JS、CSS、图片（在线工具、clean-css、UglifyJS、imagemin）
    3. 开启Gzip（压缩技术方案）（最高能压缩90%，减少了带宽）
        * 浏览器发送请求头时，加accept-encoding：gzip
        * 服务器进行Gzip编码（压缩）
        * 服务器的响应头，加content-encoding：gzip
        * 浏览器进行解码（解压），在浏览器上显示
* `提高速度`：提高网络传输
    1. 使用浏览器缓存
        * 减少冗余的数据传输
        * 减少了服务器的负担
    2. 使用CDN内容分发网络（content delivery network）
        * 缩短传输路径，就近访问