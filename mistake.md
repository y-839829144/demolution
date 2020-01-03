## 

### context.all() 在checkout.py

所传后面路径不明确

payments = context.call('get/ppayment/get_online_payments')['objects']

get/ppayment/get_online_payments 获取在线支付方式

```js
dw.fetch('get/ppayment/get_online_payments')
```

### context.get_request_meta()['useragent']

获取请求元数据

```
request_meta = {
	"ip":self.get_creator_ip(),
	"useragent":self.get_useragent(),
	"referer":self.get_referer(),
}
```



查找未找到的接口去API文档里面去找

 OrderDeliveryRepo()

post/orderdelivery/get_trace_by_tracking_number 查询物流信息

require ["siteuser","related_company"]

expresscompany_code   必填 默认值 BLANK 物流公司编码

tracking_number   必填  运单号码

mobile    发件人或者收件人手机号

client_code   顾客编码

check_word  校验码



ExpressCompanyRepo()

查询物流信息



slug

```python
class Article():
    title = models.CharFiled(max_length=100)
    content = models.TextField(max_length =1000)
    slug = models.SlugField(max_length=40)
  
上面是一个Django的Model,包含三个字段,最后一个是Slug字段,一般的,我们想显示一篇Article,常用的方式可能是
www.example.com/article/3/
www.example.com/a_id/3/
或者,会弱化你的URL可读性,你可能会希望你的URL能使用title字段或content字段的部分内容来构建起来:
    www.example.com/article/America attack on lraq
    URL不能识别出空格,真实路径会变成
    www.example.com/article/America%20attack%20on%20lraq
    在你其他地方,会变得无法识别,你就需要将这个空白符进行转义,
    www.example.com/article/America-attack-on-lraq
    这个过程就是Slug
```

