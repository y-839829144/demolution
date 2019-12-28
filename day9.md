## 结算页

商品名称

商品图片

商品价格

商品数量

收货人姓名

手机

确认购买按钮

```javascript
//input框
		<wx-input
              class="message-input b-ddxx-value"
              placeholder="点击给商家留言"
              :value="slide.dwdata.checkData.remark"
              @update:value="updateInput($event, 'remark')"
              dw-event="bind:input:updateInput"
              data-string-value="remark"
            ></wx-input>
```



## 订单详情页

订单编号

商品编号

商品名称

商品图片

商品价格

商品数量

收货人姓名

手机



```
失误:
在列表展示的时候,忘记函数是一个异步函数,没有写await,导致返回一个promise对象,无法进行页面展示

```

```javascript
云函数
event参数

```

页面跳转

gohref(' ',{ }) 前面的参数是表示页面的slug

小程序是通过slug进行页面跳转的

## 云函数

注意:

```
一般在云函数中通过repo = context.get_rows_repo(table_name)获取某个商家某个应用下某个表的所有数据
上述repo即为DatasetQueryRepo的实例对象
大部分接口都有同步和异步版本,建议优先使用异步版本

__ailter__()
遍历自身,异步版本
# example
 async for entity in repo:
 		context.log(entity)
 Q(**kwargs)-->shared.dataset.dqr.Q
 进行逻辑查询包装对象
 一般配合 repo.filter(repo.Q(data__name='test'))使用
 active(*args,**kwargs)-->shared.dataset.dqr.DatasetQueryRepo
 过滤出未被删除的数据
 (对于有删除功能的数据表,几乎都需要这个步骤)
```

