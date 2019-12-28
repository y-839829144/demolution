## 调样式

1. 方法不同:

   和之前的调样式不同,之前是通过写死的样式,而这里用的是活的样式

   也就是样式可变,数据没有写死

   通过在自定义设置里面,可以选择设置多个组件并在xcss和xhtml中进行调用,这样就可以很轻松的随时修改样式

2. 遇到了一些问题:

   关于尺寸的问题,尤其是边框 ,改了数据自己就又跳回去了.

   纠结了很久,也没纠结出多大样子来,老方法给它搞下来.

3. 学了一种border变色

   原理的嘛就是修改每条边框的内外颜色以及边距

   选择自己的border然后选择box-shadow,这样我们就可以进行样式的调样

   

   ```
   来一个内嵌:
   box-shadow: 0px 0px 10px red inset
   ```

4. CSS点击时变色，点击其他时自动还原可以通过 锚伪类实现。属性有：a:link、a:visited、a:hover、a:active。

   1. 锚伪类概述：在支持 CSS 的浏览器中，链接的不同状态都可以不同的方式显示，这些状态包括：活动状态，已被访问状态，未被访问状态，和鼠标悬停状态。

   2. 对浏览器的支持：所有主流浏览器都支持 :link、visited、hover、active选择器。

   3. 实际案例：

      A、link案例：

      `a:link{ ``background-color``:``blue``;}`

      B、visited案例：

      `a:visited{ ``background-color``:``#FF0000``;}`

      C、hover案例：

      `a:hover{ ``background-color``:``red``;}`

      D、active案例：

      `a:active{ ``background-color``:yellow;}`

      E、综合案例：

      `a:link {``color``:``#FF0000``}   ``/* 未访问的链接 */``a:visited {``color``:``#00FF00``} ``/* 已访问的链接 */``a:hover {``color``:``#FF00FF``}      ``/* 鼠标移动到链接上 */``a:active {``color``:``#0000FF``}  ``/* 选定的链接 */`

      F、提问中的案例代码：

      `a:link {``color``:``#0000``}    ``/* 未访问的链接 */``a:visited {``color``:``blue``}     ``/* 已访问的链接 */``a:hover {``color``:``#0000``}     ``/* 鼠标移动到链接上 */``a:active {``color``:``#0000``}     ``/* 选定的链接 */`

      注：根据实际需要自己修改颜色即可。

   4. 注意事项：

      A、在 CSS 定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。

      B、在 CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。

      C、伪类名称对大小写不敏感。

      D、visited 选择器对指向已访问页面的链接设置样式，:hover 选择器用于设置鼠标指针浮动到链接上时的样式，:active 选择器用于设置点击链接时的样式

5. 实现分割线

   ```css
   
   .demo_line_01{
       padding: 0 20px 0;
       margin: 20px 0;
       line-height: 1px;
       border-left: 200px solid #ddd;
       border-right: 200px solid #ddd;
       text-align: center;
   
   //如果你需要分割块级元素的话,可以直接在中间给他们添加一个空的div,通过这个div去分割两个div
      可以在css中修改这个div的样式
   ```











管理界面:

1. 产品的分类

   构建一个产品分类表,关联产品表,进行分类

2. 产品的增加删除修改

   写增删改接口

3. 订单的增加删除修改

   写增删改接口

购物车支持

将多个商品添加到一个列表里,并进行价格的统计

配置微信支付支持

收货地址的添加



系统订单支持



支付支持



订单超时自动取消







