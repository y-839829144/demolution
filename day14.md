```js



/* global dw:true */
/* build:dwapp:remove */
/* build:include my_fixture.js */
/* build:dwapp:remove */
let FIXTURE
export default {
  data: {
    cart_uuid: '',//购物车ID
    price: 0,//总价
    images: [],//图片列表
    loading: false,//加载中
    refreshing: false,//刷新中
    items: [],//产品列表
    platform: dw.platform, // 运行平台
    totalCount: 0,//总数
    allpick: false//全选
  },
  onLoad () {
  },
  //页面渲染时调用的函数
  onShow () {
    dw.methods.init()
  },
  // 下拉刷新 然后调用产品加载函数
  async onPullDownRefresh () {
    dw.setData('refreshing', true)
    await dw.methods.loadItems()
    dw.wx.stopPullDownRefresh()
  },
  //达到底部  判断列表长度是否小于产品总数量 如果小于 调用加载函数
  onReachBottom () {
    if (dw.data.items.length < dw.data.totalCount) {
      dw.methods.loadItems()
    }
  },
  methods: {
    async init () {
      // 将刷新从false改为true
      dw.setData('refreshing', true)
      // 调用产品加载函数
      await dw.methods.loadItems()
      //调用价格修改函数
      dw.methods.updatePrice()
    },
    // 全选函数
    async doAllPick () {
      //定义一个res 来接收 云函数 do_allpick的返回值
      const res = await dw.app.run('do_allpick', {
        //此时的allpick是目前产品数据allpick的相反值
        allpick: !dw.data.allpick
      })
      // 如果res返回成功 调用 init()函数 重新对页面进行渲染
      if (res.status === 'success') {
        dw.methods.init()
      }
    },
    //修改价格函数
    async updatePrice () {
      //声明 price为0
      let price = 0
      //打印产品数组
      console.log(dw.data.items)
      //将产品数组中的每一项做判断 如果每一项被选择 则进行下一步计算 价格
      dw.data.items.forEach(i => {
        if (i.data.selected) {
          price = i.data.price * i.data.count + price
        }
      })
      // 修改总价
      dw.setData('price', price)
    },
    //  产品选择
    async selectItem (event) {
      //声明item 通过三元运算符判断运行平台是否是web 是则 item = event 否则 item = event.currentTarget.dataset.bindValue
      let item = dw.platform === 'web' ? event : event.currentTarget.dataset.bindValue
      //将item的selected 属性 转换
      item.data.selected = !item.data.selected
      // 调用修改订单产品的函数 参数为item
      await dw.methods.updateOrderItem(item)
      // 调用init函数重新渲染页面
      dw.methods.init()
    },
    // 修改订单产品
    async updateOrderItem (item) {
      // 调用 update_orderitem 云函数
      await dw.app.run('update_orderitem', {
        //参数为 将item的uuid赋值给 订单产品的uuid,data就是 itme的data
        orderitem_uuid: item.uuid,
        data: item.data
      })
    },
    //加载产品 
    async loadItems (init = false) {
      // 声明allpick为true
      let allpick = true
      // 声明 api  = 云函数
      const api = dw.data.currentTab = 'get_orderitems'
      dw.setData('loading', !dw.data.refreshing)
      // 调用系统函数 显示加载中消息提示框
      dw.wx.showLoading({
        title: '正在加载...'
      })
      //声明一个res 来接收 产品的数据 如果是web就用自己的FIXTURE 如果不是就运行上面的被赋值的api
      const res = dw.platform === 'web' ? FIXTURE : await dw.app.run(api, {
        limit: 10,
        offset: dw.data.loading ? dw.data.items.length : 0,
        filters: dw.data.filters
      })
      //调用系统函数 隐藏消息提示框
      dw.wx.hideLoading()
      console.log('cart')
      console.log(res)
      if (res.status === 'error') {
        return dw.alert('错误', res.message)
      }
      // 声明objects 是 拿到产品订单的每一项数据 进行判断 如果不是购物车的id则将购物车的id改为 订单产品中的购物车id 
      const objects = res.objects.map(i => {
        if (!dw.data.cart_uuid) {
          dw.setData('cart_uuid', i.data.cart_uuid)
        }
        //在对每一项是否被选择进行判断 如果被选择 allpick改为false 返回每一项订单产品的数据
        if (!i.data.selected) {
          allpick = false
        }
        return i
      })
      // 修改值 
      dw.setDatas({
        allpick,
        refreshing: false,
        loading: false,
        totalCount: res.meta.total_count,
        //concat方法 用于数组的添加
        items: dw.data.loading ? dw.data.items.concat(objects) : objects
      })
    },
    // 结算 先判断购物车内是否有商品
    goCheckout () {
      if (!dw.data.items.length) {
        dw.alert('购物车内没有产品')
        return
      }
      //跳转到结算页面 参数为购物车uuid
      dw.goToHref(dw.app.buildPappUrl('checkout', { cart_uuid: dw.data.cart_uuid }))
    },
    // 数量减少函数 先声明 item 然后三元运算给item赋值 之后在判断 数量是否不等于1  调用 修改订单产品的函数 重新加载页面

    async doMinus (event) {
      let item = dw.platform === 'web' ? event : event.currentTarget.dataset.bindValue
      if (item.data.count !== 1) {
        item.data.count = item.data.count - 1
        await dw.methods.updateOrderItem(item)
        dw.methods.init()
      }
    },
    // 数量增加函数  先声明 item 然后三元运算给item赋值  或运算 调用修改订单产品的函数 重新加载页面
    async doPlus (event) {
      let item = dw.platform === 'web' ? event : event.currentTarget.dataset.bindValue
      if (!item.data.product_uuid__toone.data.stock || item.data.count <= item.data.remain) {
        item.data.count = item.data.count + 1
        await dw.methods.updateOrderItem(item)
        dw.methods.init()
      }
    },
    //移除 商品  先声明 item 然后三元运算给item赋值 调用产品订单的云函数  参数传为item的uuid 成功重新加载页面
    async removeItem (event) {
      let item = dw.platform === 'web' ? event : event.currentTarget.dataset.bindValue
      const res = await dw.app.run('remove_orderitem', {
        orderitem_uuid: item.uuid
      })
      if (res.status === 'success') {
        dw.methods.init()
      }
    }
  }
}

```

```
pc端跳转用event,小程序则用event.currentTarget.dataset.bindValue

如何获取当前用户
```

1. 画一遍数据库的结构图

2. 搞清楚每个字段的作用

   charge_id  

   order_id

   系统表里面有什么不清楚

3. 搞清楚每一条记录是如何创建的(每个表的每一条记录)

   分类记录:

   管理界面新增分类删除分类修改分类查看分类

   产品记录:

   管理界面新增产品删除产品修改产品查看产品

   购物车记录:

   详情页里点击加入购物车

   订单产品记录:

   通过点击加入购物车

   订单记录:

   通过点击立即支付

4. 搞清楚所有的云函数

   repo.active():过滤出未被删除的数据

   apply_pagination(limit=10,offset=0,select_related=None):

   [同步]返回分页数据并进行同步关联查询

   async_tool

   to_dict():转换到Python字典形式,如果需要输出额外的字段,可以重写中额个方法,额外的字段必须以_开头

   Dapi注入

   一个相对标准的Dapi声明如下:

   * 定义号入参  get_schema/post_schema 和 出参 return_schema

   * 确定基础权限 async_require

   * 定义文档 第一行以空格起始书写标题,其他行书写详细文档

   * ```
     SCHEMA_GET_MP_ARTICLE_CONTENT = {
     	"url":value_object.String(required=True),
     }
     SCHEMA_GET_MP_ARTICLE_CONTENT_RETURN = {
     	'status':value_object.String(required=True,description="请求状态"),
     'object':value_object.Dict({
     	'content':value_object.String(required=True,default="hello",description="公众号文章内容"),},required=True,description="请求结果"),
     
     }
     ```

     

5. 搞清楚所有的前端代码

6. 先把商城生成一个真实小程序自己用一遍

   在小程序设计,左下角有生成小程序按钮 生成后用开发者工具打开,手机扫码查看小程序