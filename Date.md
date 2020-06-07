# Date类型
## new Date()
```javascript
//代表自1970年1月1日00:00:00 (世界标准时间) 起经过的毫秒数
var today = new Date(1453094034000);
//表示日期的字符串值
var birthday = new Date('December 17, 1995 03:24:00');
var birthday = new Date('1995-12-17T03:24:00');
// year 代表年份的整数值，为了避免2000年问题最好指定4位数的年份; 如使用 1998, 而不要用 98
// month 代表月份的整数值从0（1月）到11（12月）
// day 代表一个月中的第几天的整数值，从1开始
// hour 代表一天中的小时数的整数值 (24小时制)，minute 分钟数，second 秒数，millisecond 毫秒数
var birthday = new Date(1995, 11, 17, 3, 24, 0);
```
## Date.now()
* 返回使用代码时的毫秒数
* IE8-不兼容此API则使用`+new Date()`
## Getter
1. `getFullYear()`：根据本地时间返回指定日期对象的年份（四位数年份时返回四位数字）
2. `getMonth()`：根据本地时间返回指定日期对象的月份（0-11）
3. `getDate()`：根据本地时间返回指定日期对象的月份中的第几天（1-31）
4. `getDay()`：根据本地时间返回指定日期对象的星期中的第几天（0-6）
5. `getHours()`：根据本地时间返回指定日期对象的小时（0-23）
6. `getMinutes()`：根据本地时间返回指定日期对象的分钟（0-59）
7. `getSeconds()`：根据本地时间返回指定日期对象的秒数（0-59）
8. `getMilliseconds`()：根据本地时间返回指定日期对象的微秒（0-999）
9. `getTime()`：返回从1970-1-1 00:00:00 UTC（协调世界时）到该日期经过的毫秒数，对于1970-1-1 00:00:00 UTC之前的时间返回负值。

## dateFormat实现(将时间格式化为：YYYY-MM-DD HH:mm:ss)
```javascript
// 小于或等于9的数字前面添加0
function addZero(num) {
    return num > 9 ? num : '0' + num;
}

// 格式化时间，可传入一个时间或使用当前时间
function dateFormat(date) {
    var date = date ? new Date(date) : new Date();
    var str = date.getFullYear() + '-' + addZero(date.getMonth() + 1) + '-' + addZero(date.getDate()) + ' ' + addZero(date.getHours()) + ':' + addZero(date.getMinutes()) + ':' + addZero(date.getSeconds());

    return str;
}
```

## Setter
1. `setFullYear()`：根据本地时间为指定日期对象设置完整年份（四位数年份是四个数字）
2. `setMonth()`：根据本地时间为指定日期对象设置月份
3. `setDate()`：根据本地时间为指定的日期对象设置月份中的第几天
4. `setHours()`：根据本地时间为指定日期对象设置小时数
5. `setMinutes()`：根据本地时间为指定日期对象设置分钟数
6. `setSeconds()`：根据本地时间为指定日期对象设置秒数
7. `setMilliseconds()`：根据本地时间为指定日期对象设置毫秒数
8. `setTime()`：通过指定从 1970-1-1 00:00:00 UTC 开始经过的毫秒数来设置日期对象的时间，对于早于 1970-1-1 00:00:00 UTC的时间可使用负值。