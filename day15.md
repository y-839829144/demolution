## 今天搞不完购物车 不回家! 不回家! 不回家!

## 搞清楚所有的云函数

```python
async_count:统计查询集的数量
python__str__()方法
当使用print输出对象的时候,只要自己定义了__str__(self)方法,那么就会打印在从这个方法中 return的数据
__str__方法需要返回一个字符串,当作这个对象的描写
apply_pagination	[同步] 返回分页数据并进行同步关联查询
async_apply_pagination	返回分页数据并进行并发关联查询
Parameters:	
limit – 分页限制数量
offset – 分页偏移量
select_related – 关联查询配置
select_related.toone – 外键
select_related.tomany – 反向外键
select_related.manytomany – 多对多关系
return_type – 返回格式: default: 默认格式; first: objects 里的第一个条目
fields – 限制返回的字段(注意必须包括 id/uuid 和外键字段)
aggs – 聚合查询支持
select_related 支持的功能：

toone 一对一
核心语句是 {foreignkey_field_name}__{foreignkey_table}
em:category_uuid__category
@app.register_func()
async def test_toone(event, context):
    repo = context.get_rows_repo('post')
    response = await repo.async_apply_pagination(select_related={
        "toone": {
            "category_uuid__category": {}  # 注意这里
        }
    })
    return response

tomany 一对多
核心语句是 {foreignkey_table}__{foreignkey_table_foreignkey_name}
em:comment__post_uuid
@app.register_func()
async def test_tomany(event, context):
    repo = context.get_rows_repo('post')
    response = await repo.async_apply_pagination(select_related={
        "tomany": {
            "comment__post_uuid": {}  # 注意这里
        }
    })
    return response
toone_pitem 一对一个系统表
toone_pitem 是为了解决关联查询系统表设计的，与 toone 的区别在于查询语句需要标记 pitem， 具体格式为 {foreignkey_field_name}__pitem_{foreignkey_table}
em:siteuser_id__piem_siteuser
@app.register_func()
async def test_toone_pitem(event, context):
    repo = context.get_rows_repo('comment')
    response = await repo.async_apply_pagination(select_related={
        "toone": {
            "siteuser_id__pitem_siteuser": {}  # 注意这里
        }
    })
    return response



属性:
i['data']['count']


admin_category_list
查看分类列表
repo获取对应表的数据集
repo.active() 过滤出未被删除的数据集
repo.order_by()传一个排序的参数
apply_pagination 返回分页数据并进行同步关联查询

admin_category_create
data=event['data'] 获取前端传过来的数据
repo获取对应表的数据集
entity = repo.from_dict()
[同步]从字典数据创建 DatasetRowEntity
repo.create(entity)
[同步] 根据 DatasetRowEntity 创建数据库记录

entity.to_dict() 将数据转换为字典格式

admin_category_update
repo 获取对应表的数据集
repo.get(id=event['id']) 筛选出前端传回来的id行数据
event['data'] 前端传过来的数据
repo.update(entity)[同步] 更新 DatasetRowEntity 对象


admin_category_delete
repo 获取对应表的数据集
repo.get(id=event['id']) 获取前端传来的id值并赋值给entity
repo.delete(entity) [同步] 删除 DatasetRowEntity 对象


admin_order_list
repo 获取对应表的数据集
async_apply_pagination 异步返回分页数据并进行并发关联查询
limit 分页限制量 offset分页偏移量

admin_order_detail
object_id = event.get('id') 将前端传来的id值赋值给object_id
object_uuid = event.get('id') 将前端传来的id值赋值给object_uuid
filter_kargs = {} 定义一个空字典
如果获取的是object_id 
filter_kargs['id'] = object_id
如果获取的是object_uuid
filter_kargs['uuid'] = object_uuid
repo 获取对应表的数据集
select_related = {"toone":{}} 一对一
response 
=await(repo
      .active() 返回未被删除的数据
      .filter(*filter_kargs) 以filter_kargs里的数据进行过滤
      .async_apply_pagination) 返回分页数据并进行并发关联查询
      
      
      
如果 response['objects']:
obj = response['objects'][0] 将response['objects']里面第零条数据赋值给obj
addressbook_id = obj['data'].get('addressbook_id')
从obj里面的data中获取收货地址id

objdata中orderitems = (await get_system_orderitems({'oreder_id':obj['data']['order_id']},context))['objects']
get_system_dataset(db_name) → shared.dataset.dqs.DataQueryset
获取系统数据库 DataQueryset 对象
如果是 addressbook_id就执行
addressbook_entity = AddressBookrepo().get(id = addressbook_id)
获取对应id的收货地址行数据
如果是收货地址数据:
将addressbook_entity变成字典形式 赋值给obj['data']['addressbook_id_toone']
return obj

admin_product_list
repo 获取对应表的数据集
filters 过滤是 前端传过来的 filters条件
repo.active() 将未删除的数据集通过.filter过滤 order_by排序通过('data__order')排序
return repo.async_apply_pagination(limit=event.get('limit',10),offset=event.get('offset',0))
返回分页数据并进行并发关联查询 limit是前端传来的 如果没传以10走
offset 分页偏移量 如果没传 以0走

admin_product_create
data = event['data'] 将前端传来的data赋值给 data
repo获取对应表的数据集
repo.from_dict(data)
从字典数据创建 entity
repo.create
根据 entity 创建数据库记录
return entity.to_dict() 返回entity字典形式

admin_product_update
repo获取对应表的数据集
entity接收repo.get(id = event['id']) id为前端传来的id的行数据
entity.data = event['data'] entity的data接收前端传来的data
repo.update(entity)
更新entity对象

admin_product_delete
repo 获取对应表的数据集
entity 接收id为 前端传来id的行数据
repo.delete(entity)
删除entity对象

def load_list_items
page_params 接收 前端传来的page_params
datasettable 接收 page_params.get('datasettable')
limit 接收 page_params.get('limit')
offset 接收 page_params.get('offset')
order_by 接收 page_params.get('order_by',['-created'])
filters 接收 page_params.get('filters',{})
filters_by_toone = page_params.get('filters_by_toone')
default_select_related = {"toone":{}} 支持的功能一对一

select_related 接收 page_params里的 select_related,如果没有用default_select_related
apply_args = {}
接收 分页限制数量 分页偏移量 所支持的功能
dataset_repo=context.get_rows_repo(datasettable).filter(*filters).active()
context调用系统接口 获取 datasettable的数据集并按filters条件过滤出未被删除的数据集赋值给dataset_repo

如果是filters_by_toone:
将 obj_dict 在 filters_by_toone 中循环 并将每个子项赋值给 obj_dict
field 接收 obj_dict 里面获得的('field')
table_name 接收 obj_dict 里面获得的('table_name')
filter_field 接收 obj_dict 里面获取的('filter_field')
filter_value 接收 obj_dict 里面获取的('filter_value')
filter_kwargs 接收
"%s" % filter_field:filter_value  等价于
"filter_field":filter_value
'is_active':True
dataset_repo 外键关系查询字段(field,table_name,**filter_kwargs)
filter_by_toone(field, table_name, *args, is_active=True, **kwargs)
[同步] 外键关系查询

res 接收 dataset_repo 通过(*order_by)排序条件 (*apply_args)分页条件之后的数据

get_siteuser
siteuser_entity 接收 context对象调用系统async_get_siteuser_entity()接口获取登录用户数据
result 接收 siteuser_entity 字典形式
如果 siteuser_entity 获取到:
    cart_entity = get__or__create_cart(siteuser_entity.id,context)
    cart_entity 接收 获取或创建购物车之后的返回值
    orderitem_repo 接收 context对象调用系统get_rows_repo接口获取('orderitem')的数据集 获取订单产品
    orderitem_count 接收 orderitem_repo过滤出 购物车id为用户的购物车id 然后统计查询集的数量
    result.update({
        'orderitem_count':orderitem_count
    })
    将获取的orderitem_count赋值给result里的orderitem_count
    返回 'Object'的值为 result

    
    
on_order_send
system_order 接收 事件中的order
order_repo 接收 context对象调用系统get_row_repo接口获取的数据集
order_entity 接收 order_repo过滤出 订单id为前端传来的id的第一个实例对象
如果获取到 order_entity
则将order_entity.data 中 选择改为 send
然后将order_entity对象通过update函数进行更新


on_order_pay
"砍价成功后价格为0元的订单更新为pay"
system_order 接收 前端传来的order
order_repo 接收 context对象 调用get_row_repo('order') 接口获取的数据集
order_entity 接收 order_repo 过滤出订单id为前端传来的id的第一个实例对象
如果获取到 order_entity
则将order_entity.data中的选择改为 tosend
然后将order_entity对象通过Update 函数进行更新

on_order_success
system_order 接收 前端传来的order
order_repo 接收 context对象调用 get_rows_repo('order')接口获取到的数据集
order_entity 接收 order_repo 过滤出订单id为前端传来的id 的第一个实例对象
如果获取到 order_entity
则将order_entity.data中的选择改为 success
然后用Update函数更新entity对象

on_order_cancel
system_order 接收 前端传来的order
order_repo 接收 context对象调用 get_rows_repo('order')接口获取到的数据集
order_entity 接收 order_repo 过滤出订单id为前端传来的id 的第一个实例对象
如果获取到 order_entity
则将 order_entiy.data 中的选择改为 closed

remove_orderitem
data 接收 value_object.value.parse_data() 通过schema 获取前端传来的值
orderitem_repo 接收 context 对象调用 get_rows_repo('orderitem')的数据集
orderitem_entity 接收 orderitem_repo过滤出 uuid为data中的订单产品的uuid的第一个实例对象
将orderitem_entity未被删除的属性改为false
然后用update函数更新entity对象

do_allpick
siteuser_id 接收context对象调用 async_get_siteuser_entity()获取到的用户数据的id
data 接收 value_object.Value.parse_data 通过schemal获取到的数据
cart_entity 接收 get_or_create_cart(siteuser_id,context) 获取用户的购物车
orderitem_repo 接收 context 对象调用get_rows_repo('orderitem')接口获取的数据集
orderitems 接收orderitem_repo过滤出 购物车id为 传来的购物车id的字符串形式并返回指定的 uuid 和 data
遍历orderitems 
并将orderitem_entity 接收 每个 orderitem_repo过滤出uuid等于i[uuid]的第一个实例对象
将orderitem_entity.data中selected的值改为 data['allpick']的值
orderitem_repo调用async_update 更新entity对象

get_cart_price
siteuser_id 接收 context 对象调用 async_get_siteuser_entity()接口
cart_entity 接收 用户的购物车数据
orderitem_repo 接收 context对象调用get_rows_repo 获取的数据集
orderitems 接收 orderitem_repo 过滤出用户购物车的未被删除的数据并且返回指定的字段data
令price = 0
用i遍历orderitems
price =每一项的数量*价格的和

get_cart_count
cart_entity 接收 用户的购物车数据
orderitem_repo 接收 context对象调用get_rows_repo 获取到的数据集
返回orderitem_repo过滤出购物车Id的查询集数量

get_or_create_cart
cart_repo 接收 context 调用get_row_repo获取的数据集
cart_entity 接收 cart_repo过滤出用id的第一个实例对象
如果没有cart_entity
cart_entity 接收 cart_repo 从字典中创建数据库字段
cart_entity 接收 创建或保存的cart_entity
返回 cart_entity


def add_to_cart

count = 1 
siteuser_id 接收context 通过async_get_siteuser_id方法获取的用户id
orderitem_repo 接收 context对象 通过get
```



## 搞清楚所有的前端代码

```
dw.wx:
小程序运行时API宿主对象
在小程序中通过dw.wx访问
dw.wx.previewImage()
图片预览

option.current 并不会让其显示对应内容,真实显示效果请以小程序为准.
vant兼容过程中本应该支持的滑动功能无法实现,甚至点击关闭的功能也是monkey patch绕过的
dw.wx.showToast()
显示消息提示框
行为基本和小程序一致
一个比价明显的区别在于小程序浮动窗口可叠加
下面的showLoading类似

dw.wx.showLoading()
显示加载中消息提示框
dw.wx.hideToast()
隐藏消息提示框
dw.wx.hideLoading()
隐藏加载中消息提示框
dw.wx.showModal()
显示模态对话框

和小程序的区别在于按钮颜色设置
confirmColor/cancelColor 不会生效,另外样式有一些区别,但是主要的交互逻辑和小程序是一致的,有以下特点
用户点确定调用是success 参数是{errMsg:'showModal:ok',cancel:false,confirm:true}
用户点取消调用回调是success 参数是 {errMsg:'showModal:ok',cancel:true,confirm:false}



Array.forEach()
对数组的每个元素执行一次提供的函数
const array1 = ['a','b',c]
array1.forEach(element => console.log(element));


Array.map()

const array1 = [1,4,9,16]

const map1 = array1.map(x => x*2);

console.log(map1)
```



## 先把商城生成一个真实小程序自己用一遍 

## 思路:先做一个猜想,然后去找依据去验证猜想是否成立

## 把一个事情尽可能去化解成小问题或者多个小问题,然后一步步去解决

## windows 开发工具下载 自己跑一遍这个流程

## 小程序跑完把每个页面具体做什么事情有什么功能要清楚了,之后在明天结算页面的完善,订单的完善

