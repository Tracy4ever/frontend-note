# BFC(block formatting context)
* block-level box: display 属性为 block, list-item, table 的元素，会生成 block-level box。并且参与 block fomatting context；
* inline-level box: display 属性为 inline, inline-block, inline-table 的元素，会生成 inline-level box。并且参与 inline formatting context；
* run-in box: css3 中才有， 这儿先不讲了。

1. 触发BFC
* 1. body根元素
* 2. float的值不是none。
* 3. position的值不是static或者relative。
* 4. overflow的值不是visible。
* 5. display的值是inline-block、table-cell、flex、table-caption或者inline-flex。

2. BFC特点（作用）
    * 避免margin合并（塌陷，重合）
        * 重合则只写一个margin（否则改变html结构不合适）
    * 可以包含浮动元素，子元素浮动的时候，父元素没有内容撑起，此时清除浮动变成BFC则可包含浮动内容（overflow：hidden，清除浮动）
    * 使元素不被浮动元素覆盖（两栏布局）
        * 利用BFC特性：使元素不被浮动元素覆盖
        ```html
        <div style="width: 200px;height: 200px;float: left"></div>
        <div style="width: 200px;height: 200px;overflow: hidden"></div>
        ```

3. BFC的布局规则

* 内部的Box会在垂直方向，一个接一个地放置。
* Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。
* （每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。）？
* BFC的区域不会与float box重叠。
* BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
* 计算BFC的高度时，浮动元素也参与计算。

4. margin塌陷
* 父子元素的margin合并，子元素的margin-top比父元素的margin-top大，所以带着父元素塌陷
* 外层margin相对浏览器，内层margin相对父元素，但由于margin塌陷，父元素也掉下来了。`父盒子里面没有文字，也没有边框（border），也没有padding-top，`然后子元素具有margin则会出现上述情况
* 解决：触发父元素BFC解决margin塌陷

4. margin合并
* 兄弟元素发生margin合并，只设置上元素的margin-bottom/只设置下元素的margin-top
* 把兄弟元素分别放在BFC中（不建议，会改变html结构）