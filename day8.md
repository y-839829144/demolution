## 实现小程序基础 代码问题

## V-for 循环

```javascript
<wx-for v-for="good in slide.dwdata.goods" for-item="good" for-items="slide.dwdata.goods" for-key="id" class="b-good-good">

```

注意:

* slide.dwdata加js.data中定义的元素
* 否则会报 data中元素未定义

## '?'问题

```javascript
const a= 2 

b = a ==1 ? 2 : 3

```

# 小程序 WXS 支持

```javascript
// wxml

// 引入模块
<wx-wxs module="dwtools"></wx-wxs>

// 调用
<wx-image :src="dwtools.qiniu(slide.dwdata.value, 'e,200,200')" mode="widthFix"></wx-image>


// qiniu插件 
<wx-wxs module = "dwtools"></wx-wxs>

<wx-image class="" :src="dwtools.qiniu(slide.dwdata.value,'e,200,200')"

mode = "widthFix"></wx-image>


示例
<wx-image
    class="b-item-img"
   	:src="dwtools.qiniu(item.data.image,
	slide.css_ctx.fop1.value)"
  	mode="widthFix" style="width: 100%;"
    ></wx-image>
fop1 在自定义选项里面 平铺视图 然后高级选项里有个图片裁剪

支持的过滤器
qiniu
dateformat
<wx-wxs module="dwtools"></wx-wxs>

<wx-view>{{ dwtools.qiniu('fk') }}</wx-view>
<wx-view>{{ dwtools.dateformat('2018-03-29T06:22:50.107', 'datetime') }}</wx-view>
<wx-view>{{ dwtools.dateformat('2018-03-29T06:22:50.107', 'date') }}</wx-view>
<wx-view>{{ dwtools.dateformat('2018-03-29T06:22:50.107', 'time') }}</wx-view>

//https://s2.d2scdn.com/fk
//2018-03-29 06:22
//201-03-29
//06:22
```

```
v-if
<wx-view v-if="slide.dwdata.goods.id">
```

## 云函数接口问题

如果你想要一个对象而不是数据的话

```python
@app.register_func()
def goods_detail(event,context):
    repo = context.get_rows_repo('goods')
    # uid = event['uuid']
    uid = event.get('uuid', '')
    res = repo.active().get(uuid = uid).to_dict()
    # res = entity.data
    return {
        "status": "success",
        "object": res
    }
```

## repo 的用法

```

```

## 最重要的 英文去查找 

## setDatas

```
dw.setDatas({
  'list1__concat': [1, 2, 3],
  'list2__uconcat': {field: 'id', list: [1, 2, 3]},
  'list3__push': 1,
  'dict__update': {
    a: 1,
    b: 2
  }
})

因为对于数组和对象的操作非常频繁，而小程序操作大量数据有性能问题，所以引入上述字段注解， 简化操作的同时优化性能。
支持的注解形式：

<field>__concat 直接在数组字段末尾 concat 另一个数组
<field>__uconcat 和 <field>__concat 类似，但是通过传入一个唯一性字段保证不重复
<field>__push 尾部插入一个元素
<field>__update 针对对象类型的字段更新部分字段
<field> 和以前一样支持 . 操作和索引操作，下列写法都是合理的：

field1__push
field1.field2.field3__contact
field1[0]__update
dw.setData(key, value) 同样支持
```

