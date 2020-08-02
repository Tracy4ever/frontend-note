# 安全
## 1. cookie
### 1.1 cookie的属性(每对cookie都有这些属性)
1. Domain="ke.qq.com"||"Domain=.qq.com"（只能设置上级域名，四级域名只能设置成二级或者三级域名）
2. Expires
3. cache-control的时间Max-Age=36000（Max-Age在Ie8、7、6不支持）
    * session cookie（临时cookie，关闭浏览器则失效）
    * permanent cookie（持久cookie，有效时间为Expires或cache-control时间内）
4. Path=/ 路径
5. Secure 只在https下才发送
6. HttpOnly 不能通过JS的document.cookie访问

### 1.2 操作cookie
* `获取cookie`
```javascript
function getCookie(name) {
    var name = name + '='; 
    var decodeCookie = decodeURIComponent(document.cookie);
    var ca = decodeCookie.split(';');
    ca.map(c => {
        if(c.charAt(0)===' ') {
            c = c.substring(1);
        }
        if(c.indexOf(name)===0) {
            return c.substring(name.length,c.length);
        }
    })
    return '';
}
```
* `增加cookie`
```javascript
document.cookie = encodeURIComponent('name') + '=' + encodeURIComponent('value') +
';domain=ke.qq.com' + 
';Expire='+new Date('2018-01-01');
```

* `删除cookie2`：设置超时时间为过去时间



### 1.3 http是无状态的协议
* 无状态，需要通过cookie记录登陆态
* 登陆
    1. 客户端登陆，服务器响应头返回setCookie: id=kevin;
    2. 客户端下次访问带上cookie：id=kevin
    3. 返回该登陆状态对应的数据
* 注销
    1. 客户端发送注销的请求
    2. 服务器响应头带上Set-Cookie的字段，设置超时时间为过去时间

## 2. XSS攻击（Cross-Site-Script）
* 反射型(非持久型)
    1. 黑客在链接中加入了恶意脚本（参数改为恶意脚本代码（一般会做掩饰））发送给另一个用户
    2. 用户点击了链接，向服务器发送了请求，服务器获取参数并直接使用，未做防范，向客户端“反射”结果页
    3. 客户端打开浏览了页面则受到了恶意攻击
    4. Chrome浏览器具有XSS-filter防范了一部分XSS攻击
* 存储型(持久型)
    1. 黑客发送带恶意脚本链接的评论，服务器未做处理，存储到了服务器中，被其他用户浏览（不用点击）也会触发恶性操作
* Dom-based型
    * 不经过服务器，需要诱导用户点击才会触发的攻击，也算反射型的一种
    1. 设置innerHTML，outHTML， document.write
    2. <a href=""> input设置href的时候，写了" onclick=alert(/xss/) //

### 2.1 xss-payload 有效载荷
* 能够实现xss攻击的恶意脚本
* 目的：
    1. 窃取用户cookie，模仿登录态：document.cookie
    2. 识别用户浏览器,根据不同版本浏览器用不同的方法:navigator.userAgent
    3. 伪造请求：通过img的src，ajax，formData，发送get/post请求
    4. xss钓鱼：xss payload + 钓鱼网站

## 2.2 XSS防御（js-xss）
### httpOnly
* 阻止恶意脚本访问cookie，但本地js也不能访问cookie
* 并未阻止xss攻击的发生，只是缩小的攻击范围

### 输入检查(前端和服务台)
* 永远不要相信用户的输入
    * `白名单`:输入格式判断，需符合的格式和要求：
    * `过滤危险字符`: 
        1. 去除script、javascript、onclick
        2. `黑名单`:转义特殊字符<>&\"'，使其变成文本
        ```javascript
        //转义HTML特殊字符
        function htmlEscape(str) {
            return String(str)
            .replace(/&/g,'&amp;')
            .replace(/"/g,'&quot')
            .replace(/'/g,'&#39')
            .replace(/</g,'&lt')
            .replace(/>/g,'&gt;')
        }
        ```
### 输出检查
* 最后一道防线，根据不同场景对数据进行处理
* xss payload的输出场景
    * html执行环境-> HtmlEncode()
    1. HTML标签中
    2. HTML属性中
    * js解析环境-> JavascriptEncode()//使用'\'
    3. script标签中
    4. 事件属性中
    * URL解析环境-> URLEncode()
    5. url中(src/href) encodeURI

## web相关的编码
### encodeURI
* 用于编码URI，编码一个URL，不转义保留字符和非转义字符
### encodeURIComponent
* 用于编码URI参数，不然会把url也编码了

## 3. CSRF（跨站请求伪造cross-site request forgery）
* 通过登陆的身份，伪造一个转账操作
* `特点`：用户不知情；跨站；参数伪造
### 3.1 CSRF防御
### 3.1.1 验证码
* 解决用户不知情问题
### 3.1.2 refer
* 服务端用来判断请求是否跨域，但其实可以伪造refer
### 3.1.3 token
* 参数中带上加密的token，（用cookie中的userkey进行加密处理）

## 4. XSS+CSRF蠕虫
* 页面xss注入恶意代码，恶意代码发送csrf伪造请求
* 因为同页面发送请求，refer失效，token也可能被破解

## 5. DDOS(Distributed Denial of Service分布式拒绝服务)
* 通过大量非正常请求耗尽服务器资源，使得服务无法正常进行

### 5.1 DDOS防御
* 验证码
* 提高带宽和扩容
* 限制请求频率
* 设置业务为分布式服务，防止单点失效
* 使用主流云系统和CDN服务，其自身防御DDOS
* 优化资源使用提高web server的负载能力

## 6. http
### 6.1 http与https
* http明文传播，https加密
* https是http+SSL（secure socket layer）/TLS（transport layer security）
* TLS负责`加密算法`,`密钥协商`，`服务器验证`
### 6.2 对称加密非对称加密
* 对称加密，用同一密钥解密加密
* 非对称加密，密钥成对出现，有私钥和公钥。
### https过程
1. 客户端发送https请求，服务端返回公钥。
2. 客户端生成自己对称密钥，使用公钥加密对称密钥
3. 发送加密后的对称密钥
4. 服务端使用私钥解密对称密钥
5. 服务端使用对称密钥通信，加密内容

* `弊端`：公钥明文传播，容易被截获，窃听者伪造服务端和客户端

### 改进
* CA（数字证书认证机构）
* 服务端申请证书，返回证书（包括公钥），服务端认证证书

