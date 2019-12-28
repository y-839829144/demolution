## 实现小程序基础商城



第一步:

了解小程序基础组件:

* wx-view 块级容器

  * ```
    <wx-view
    	class = "b-view"
    	:dw-class = "slide.dwdata.dynamicClass"
    ></wx-view>
    ```

* wx-scroll-view 可滚动视图区域

  * ```
    <wx-scroll-view class = "b-items" scroll-y = "true">
    </wx-scroll-view>
    ```

* 滑块视图容器

  * ```
    <wx-swiper class = "b-items" :settings= "slide.dwdata.settings">
    	<wx-swiper-item class = "b-item">1</wx-swiper-item>
    	<wx-swiper-item class = "b-item">1</wx-swiper-item>
    	<wx-swiper-item class = "b-item">1</wx-swiper-item>
    	<wx-swiper-item class = "b-item">1</wx-swiper-item>
    </wx-swiper>
    
    js
    export default {
      data: {
        lock: false,
        settings: {
          duration: 500,
          interval: 5000,
          autoplay: true,
          'indicator-dots': true
        }
      }
    }
    
    cs
    .b-items {
      height: 200pm;
    }
    ```

* wx-image 图片组件

  * ```
    <wx-image
    	class = "b-image"
    	:src = "gift.img_url"
    	mode = "widthFix"></wx-image>
    	
    ```

* wx-input 文本框

  * ```
    <wx-input
    	class = "b-input"
    	type = "text"
    	:value = "slider.dwdata.name"
    	@update:value = "updateInput"
    	dw-event = "bind:input:updateInput"></wx-input>
    ```

* wx-pick 选择器

  * ```
    <wx-pick mode="selector" :value="slide.dwdata.ruleType.value"
            @update:value="updateSelect" dw-event="bind:change:updateSelect"
            :range="slide.dwdata.ruleType.range" :range-key="slide.dwdata.ruleType.rangeKey">
            <wx-view slot="selector">
              <wx-view class="weui-select weui-select_in-select-after">
                {{ slide.dwdata.ruleType.range[slide.dwdata.ruleType.value]["name"] }}</wx-view>
            </wx-view>
          </wx-pick>
    ```

    ...扩展运算符 将一个对象展开

    列表渲染

    <viewwx:for = "{{array}}">{{index}}:{{item.message}}</view>

    <viewwx:for = "{{array}}"wx:for-index = "idx" wx:for-item="itemname">{{idx}}:{{itemname}}</view>

    block wx:for

    <block wx:for="{{[1,2,3]}}

    <view>{{index}}</view>

    <view>{{item}}</view>

    </block>

    条件渲染

    <view wx:if="{{condition}}">True</view>

    dw.setData(path,value,callback)

    path string 路径

    value string 值

    callback function 数据映射到DOM之后的回调函数



## 实现页面跳转

```js
dw.goToHref('list-product')
跳转到当前组件的子页面,使用dw.goToPage('detail',{id:item.id})
```

## 支持嵌套修改/批量更新数据

dw.SetData(key,value) 支持修改嵌套对象

```

export default {
  data: {
    key1: 1,
    obj: {
      key2: 3
    }
  },
  onShow () {
    dw.setData('key1', 2)
    dw.setData('obj.key2', 4)
  }
}

// 输出结果: { "key1": 2, "obj": { "key2": 4 } }
```

dw.SetDatas(obj) 支持批量更新数据

```
export default {
  data: {
    key1: 1,
    obj: {
      key2: 3
    }
  },
  onShow () {
    dw.setDatas({
      key1: 2,
      'obj.key2': 4
    })
  }
}

// 输出结果: { "key1": 2, "obj": { "key2": 4 } }
```

## 绑定事件

```javascript
<wx-view class="my-menu-subitem gridXt" dw-event="bind:tap:go" data-string-value="orderlist">
<wx-text class="arrowL" value="我的订单"></wx-text>
</wx-view>

go (event) {
    const pageName = dw.platform === 'web' ? event : event.currentTarget.dataset.stringValue
	dw.goToHref(dw.app.buildPappUrl(pageName))
}
```

## 搜索框查询

```

```

## 页面跳转携带参数

```javascript
goDetail (event) {
      const uuid = dw.platform === 'web' ? event : event.currentTarget.dataset.bindValue
      dw.goToHref(dw.app.buildPappUrl('detail', { uuid,id:5, }))
    },
```

