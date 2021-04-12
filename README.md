---
description: 描述
---

# 主页

## 哈哈哈内容 🍏 

```java
    public void channelReadComplete(ChannelHandlerContext ctx) throws Exception {
        this.resetReadAndFlushIfNeeded(ctx);
        ctx.fireChannelReadComplete();
    }
```

{% api-method method="get" host="https://www.baidu.com" path="/getUserInfoByUserId" %}
{% api-method-summary %}
 获取个人信息
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="scope" type="string" required=false %}

{% endapi-method-parameter %}

{% api-method-parameter name="userId" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{"result":{"nickname":"xxx","headurl":"xxx"}}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% hint style="info" %}
hint
{% endhint %}

![&#x529F;&#x592B;&#x718A;&#x732B;](https://n.sinaimg.cn/tech/transform/298/w185h113/20210412/3e89-knqqqmu8166123.gif)

文**本**_**!**_



