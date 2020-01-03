##  云函数

### 购物车

get_siteuser()

参数:不需要参数

获取登录用户

在详情页 getSiteUser 异步函数中调用



remove_orderitme()

参数: 订单商品的uuid

移除购物车中的某个商品

在购物车页面中调用



do_allpick()

参数:allpick的布尔值

在购物车中将所有产品选中

在购物车页面中调用



get_cart_price()

参数:不需要参数

在购物车中调用系统函数获取用户id,通过用户id找到用户的购物车id通过购物车id找到用户所加入购物车的商品信息然后计算价格

在购物车页面中调用



get_cart_count()

同上 ,找到购物车商品信息之后通过 async_count统计数据集的数量



go_or_create_cart()

参数:用户id

通过系统函数获取对应cart表数据,然后通过用户id进行筛选购物车

如果没有,则新建一个购物车

在云函数 get_cart_price,get_cart_count(),add_to_cart(),do_allpick()中调用



add_to_cart()

参数:产品的uuid和数量

通过系统函数获取用户id,通过产品id在产品表数据中筛选出对应的产品信息

之后根据用户id创建属于用户的购物车,根据购物车id,产品id找出所对应的订单商品数据,如果有订单商品则修改其数量,如果没有则调用系统函数 repo.from_dict从字典数据中创建数据库记录

在产品详情页,加入购物车时调用



### 分类

web_category_list()

参数:需要传过来排序方法

通过系统调用接口获取对应的repo数据集,然后通过传来的排序方法进行排序





### 结算

web_checkout_detail()

参数:购物车id

通过schemal获取购物车id 通过购物车id筛选出购物车数据,根据购物车id筛选出订单商品

在结算页面 async load() 加载时调用



handle_quick_buy()

参数:产品id

通过schemal获取产品id 通过产品id筛选出产品数据,通过系统接口获取用户id

在结算页面 handle_quick_buy 调用



web_checkout_submit()

参数:方式,产品id,购物车id,数量,备注,支付方式id,收货地址id,slug

会员商城流程

cart

​	验证产品是否能购买

​		is_active

​		onsale

​		remain>count

​	计算price

​	保存order

​	保存order_entity

​	更新orderitem

​		cart_uuid => None

​		order_uuid => order_entity.uuid

​	quickbuy

获取用户id,从schemal中获取data,将data中的remark state,siteuser_id,payment_id,addressbook_id赋值



​	to fill

​	price

​	charge_id

​	order_id

​	判断是否是快速购买,获取产品表数据并根据产品id筛选出产品信息

​	如果没有产品数据 则返回产品已经下架

​	如果产品还有库存,已卖数量和data的数量大于库存,则返回库存不足

​	订单商品的数据改为..

​	然后修改订单总价 

​	判断是否是购物车,获取订单商品数据集,并根据购物车id筛选出订单商品

​	之后将订单商品进行数据分页处理,定义一个价格为0的变量price,定义一个orderitems_data的空列表,for循环获取的分页数据集,获取单个商品,判断商品是否活跃或者没有上架,返回商品已经下架

库存 没有cart_uuid且有order_uuid的orderitem为已售出的商品

获取已经售出商品的数量,判断商品是否有库存并且已售数量加要买数量大于库存的话返回 库存不足,商品价格获取,然后计算总价等于商品价格x商品数

将之前定义的空列表把商品的名称数量单价总价添加进去,然后修改订单的总价 如果订单总价小于等于0,则修改订单state为tosend 获取订单表的数据集,将订单信息创建出数据记录

构造回调链接

创建订单

order_entity对象里的order_id 被返回的orderid赋值

如果返回值中的order里存在charge_id,则赋值给order_entity对象里的charge_id 然后保存创建order_entity对象

判断购买方式为cart,

获取订单商品表的repo数据集,然后根据购物车id筛选出订单商品的数据,

然后循环订单商品数据,将其里面的cartid改为空,将orderid改为order_entity对象的uuid然后repo数据集保存

判断购买方式为立即购买

快速购买者里才创建orderitem

获取订单商品表的repo对象,根据数据创建orderitem_entity对象,然后保存创建entity对象



在结算页面点击立即支付调用

### 订单

get_system_orderitems()

参数: order_id

通过schemal获取order_id,通过OrderItemRepo筛选出order_id的数据并返回

在订单详情 加载订单商品时调用



user_order_list()

参数:filters,offset,limit 可以不传,order_id

通过调用系统接口获取用户id,如果没有用户id就返回请先登录,

repo获取order表的数据集对象,过滤条件是前端传来的过滤条件,可为空

定义一个res来接收分页查询数据集,遍历res将每一项的orderitems被赋值

通过调用get_system_orderitems



user_order_detail

参数:id,uuid

获取用户id,判断是否获取到用户id 没有则返回

将用户id赋值给data__siteuser_id并打包成字典赋值给filter_kargs

判断是否获取到id,uuid并将其赋值给filter_kargs里的id,uuid

repo获取order表的数据集,定义response为返回分页查询并进行并发关联查询后的数据集

判断是否获取到数据集中的objects这项

定义obj为objects的第0项

addressbook_id 为obj data中的addressbook_id 

定义orderderlivery_entity为调用OrderDeliveryRepo()函数筛选order_id的第一项

判断是否获取到orderderlivery_entity

如果获取到将其打包成字典赋值给obj中的data下的orderdelivery

定义expresscompany_entity = ExpressCompanyRepo()函数过滤出订单交货的快递公司id的第一项

判断是否获取到expresscompany_entity

如果获取到将其打包成字典赋值给obj中的data下的expresscompany_entity

判断是否获取到收货地址id

如果获取到将其打包成字典赋值给obj中的data下的addressbook_id__toone

在订单详情页面 load 中调用

### 订单产品



get_orderitems()

参数: offset,limit

获取登录用户id,获取orderitem表的repo,根据用户id调用获得或者新建购物车

data 接收schemal传来的限制条件

然后进行分页数据查询并进行并发关联查询并将结果赋值给resp

遍历resp里的objects对象 

判断data里product_uridine__toonedata中的stock 不为0

则data中的remain 为get_remain()参数为产品id的结果

否则remain为0



在购物车页面中 loadItems()调用





update_orderitem()

参数: orderitem_uuid  data中的count selected

获取orderitem表的数据集对象

data接收schemal传来的data

根据传来的orderitem_uuid过滤出orderitem_entity对象的第一项

然后调用update函数将传来的data修改原来的data

然后保存entity对象

在购物车页面 updateOrderItem调用



## 前端页面

### 产品列表页面

功能:1. 商品的展示,(有商品的价格,名称,简介)

   2. 商品的搜索和分类筛选

   3. 点击商品跳转到商品详情页面(传递该商品的id值)

      所需调用的云函数:

      web_product_list() 获取产品列表数据

      需要传递 分类id 名字 图标

### 产品详情页面

功能:1.商品的详细信息(名称,价格,简介,详情)

2. 商品数量的选择
3. 点击立即购买跳转到结算页面
4. 点击加入购物车跳转到购物车页面,并且在数据集中创建记录

所需调用的云函数:

​		get_siteuser() 获取登录用户 如果该用户没有购物车则会新建

​		web_product_detail 获取商品详情

​		add_to_cart 将产品添加到购物车



### 购物车页面

  功能:1. 购物车里商品的信息

    2. 商品的移除 
       3. 数量的增加减少
    4. 是否被选中
    5. 全选
    6. 结算

所需调用的云函数

​		do_allpick 将商品全部选择

​		update_orderitem 修改订单产品表里的信息

​		get_orderitems 获取购物车里的商品信息

​		remove_orderitem 移除购物车里的商品

​		

### 结算页面

功能:1. 购物车内选中的商品展示

2. 添加收货人信息
3. 支付方式
4. 备注
5. 立即支付

所需调用的云函数

​			handle_quick_buy  快速购买 通过详情页直接购买而不通过购物车

​			web_checkout_detail 结算详情 产品信息  购物车

​			web_checkout_submit  结算提交 创建订单

### 订单详情

功能: 1. 收货地址的显示

	2. 订单状态
 	3. 商品信息
 	4. 支付方式
 	5. 买家留言
 	6. 商品金额
 	7. 应付总额

所需调用的云函数

​				get_system_orderitems  获取系统订单商品

​				user_order_detail    用户订单详情 包含用户支付方式,收货地址,快递公司

​				

### 订单列表

功能:1. 订单状态分类显示

所需调用的云函数

​				user_order_list  获取用户订单列表