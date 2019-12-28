## 任务规划

### 管理页面

```


系统调用app.getDcompany()
需要依赖组件
在依赖管理里面进行配置添加
appProxy.getDcompany()
获取 Webpack Entry: dcompany

Kind: instance method of AppProxy

appProxy.getVbench()
获取 Webpack Entry: vbench

Kind: instance method of AppProxy
```



表单的创建 可以在管理文件中选择 添加.form文件

在html里面调用的时候<vformFileWebRender标签

其中:params .slug就是你的表单名称所填写的地方

```

<VformFileWebRender
 ref="formCreate"
 :params="{slide: slide, slug: 'my_form_product.form',   formSlugData: defaultCreateData}"
          ></VformFileWebRender>
```

表数据的查询过滤

用<VuiAdminFilterSeclect标签,他有

:value属性 可以添加你所要判定的数据

:choices

```
<VuiAdminFilterSelect
          :value="filters.data__onsale"
          :choices="isSaleChoices"
          :append="true"
          :appendId="undefined"
          :appendName="'全部'"
          :selectStyle="{width: '100px'}"
          @input="mixinUpdateFilters('data__onsale', $event)"
        ></VuiAdminFilterSelect>
```



### 购物车

功能:

1. 商品数量的增加减少

   绑定一个事件,设置每次点击时进行判断,如果是减少的话,该值在!=1时,可以进行减1,依次循环,增加事件 如果是增加的话,每次点击 次数+1

   每次修改之后调用update函数 对数据库里的数据进行修改.

2. 商品移出购物车

   绑定一个事件,用来调用云函数,云函数用来删除orderitem,所以需要传递一个 orderitem的uuid,删除成功.

3. 价格的计算

   1. 如果 商品被选中,增加或减少数量 总价发生变化

      选中,可以给商品添加一个是否被选中的属性,如果被选中,则可以绑定个事件,然后在事件中修改是否被可选的值是 True Or False

      计算时要判断每个商品的是否被可选的值为true 则进行计算,false则不计算

      单个商品价格计算,如果商品数量增加,则它的价格会跟着增加

      price  = count * 单价

   2. 如果 商品没有被选中,增加或减少数量 总价不发生变化

      

   

   ​	

```

```

