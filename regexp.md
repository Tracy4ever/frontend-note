# 正则表达式
* `正则字面量`： var reg = /(?=(?!\b)(\d{3})+$)/g; (js中的正则，pattern/flag)
* `正则构造函数`： var reg = new RegExp('(?=(?!\\b)(\\d{3})+$','g');
* 表达对字符串的一种过滤逻辑（匹配，获取）
* `pattern`：有规则的字符集 
* `flag`：属性
## RegExp对象
### 方法
* .test()
* .exec()
### 属性
* source
* ignorecase//忽略大小写
* global//全局
* multiline//多行

## 盲点
* []为字符集，里面可以放任意字符
* .点匹配任何非转行字符，需要匹配点的时候需要转义
* 多个需转义字符放一起的时候可以只写一个转义符

## 网址的正则匹配表达式
http(s)://ke.qq.com/....
* s可以不存在
* 1:com/后面存在、不存在字符都行
* 2:com/必须有字符
1. /^https?:\/\/ke\.q{2}\.com\/$/
2. /^https?:\/\/ke\.q{2}\.com\/.+$/

## 分组和捕获
* `捕获型分组` ()
    * 引用
    ```javascript
    //\s表示空格
        var reg3 = /(jero)\slove\s(coding|girl)/.exec('jero love girl');
        var reg = reg3[1];
        var reg2 = RegExp.$1;//RegExp对象存储最近一次的捕获
    ```
    * 反向引用
        * 正则里面引用本正则的分组 /<div>.*</div>/ = /<(div)>.*<\/\1>/
* `非捕获型分组` (?:coding|girl)

## 贪婪匹配和惰性匹配
* 惰性匹配是贪婪匹配后的普通量词加一个？
* 普通量词：+*？{n}{n,}{n,m}

```javascript
var str = '<span>daklfjlkasdf</span><span>ddd</span>'
var reg = /<span>.+?</span>/
reg.test(str);//['<span>daklfjlkasdf</span>']
```

## 正向前瞻和负向前瞻(匹配位置)
* `正向前瞻`：(?=)
* `负向前瞻`：(?!)
```javascript
var reg = '/\b(\w+)(?=\.jpg)\b/g';
var str = 'img.jpg test.md hello.jpg';
str.match(reg);//[img,hello]
```
* 匹配位置的还有\b,^,$

## String对象能使用正则的方法
* `replace(reg,待替换字符串)`：一个正则匹配多种情况
* `match()`: match方法匹配所有满足的字符，RegExp.exec()只匹配第一个
* `split(reg)`：
* `search(reg)`:返回第一个index

## 例题
* 给数字10000变成10,000
```javascript
var reg = /(?=((?!\b)\d{3})+$)/g
```
