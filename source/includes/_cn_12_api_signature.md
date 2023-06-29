# 签名

API请求需要签名，以确认用户身份，并防止重放攻击。

签名算法：

针对一个HTTP的API请求，有如下信息：

* 方法名称：`GET`或`POST`
* 请求域名：例如`uniapi.876ex.com`
* 请求路径：以/开头的URI，例如：`/v1/trade/orders`
* 请求参数：以key1=value2&key2=value2形式的参数，例如：`id=123456&sort=DESC&from=2017-09-10`
* 请求Header：例如，`Accept: */*`
* 请求Body：二进制表示的JSON字符串，仅针对POST有效

然后，我们需要构造一个字符串，按如下格式填入：

```http
GET\n
uniapi.876ex.com\n
/v1/trade/orders\n
from=2017-09-10&id=123456&sort=DESC\n
API-KEY: xyz123456\n
API-SIGNATURE-METHOD: HmacSHA256\n
API-SIGNATURE-VERSION: 1\n
API-TIMESTAMP: 12300000000\n
API-UNIQUE-ID: uni-123-abc-xyz\n
<json body data>
```

注意：每行以换行符`\n`结束。最后添加的Body结尾不要添加`\n`。

第一行是方法名称，全大写的`GET`或`POST`；

第二行是请求域名，全小写，例如：`uniapi.876ex.com`；

第三行是请求路径，必须以`/`开头，严格区分大小写，最后不要写`?`；

第四行是请求参数，严格区分大小写，并按照字母顺序排序，即排序后的字符串：`from=2017-09-10`，`id=123456`，`sort=DESC`，再用`&`连接起来。如果没有请求参数，第四行是空行，注意空行也需要以`\n`结束；

紧接着把以`API-`开头的Header排序后以`HEADER: Value`的格式每行一个，Header名称全大写，冒号后面有一个空格，Value严格区分大小写，每个Header一行，以`\n`结束；

最后，如果是`POST`请求，且包含Body，把Body以JSON字符串形式添加到末尾，注意没有`\n`。如果是GET请求，或者POST请求没有Body，就添加一个空字符串`''`。

以上字符串按UTF-8编码，得到一个二进制byte数组，然后使用API Secret计算Signature：</p>

```python
signature = HmacSHA256(payload.encode("UTF-8"), "my-api-secret")
```

将所得签名以十六进制小写字符串形式添加到Header：

```http
API-Signature: a1b2c3ff001234500900dd01ff
```

### 说明

参数使用原始字符串计算签名，例如`a=1/5`，不要使用`a=1%2F5`；

Header在计算签名时使用全大写，发送时大小写均可；

只有以API-开头的Header才被列入并计算签名（API-Signature除外，因为最后才能计算出API-Signature并附加到请求）；

API-Signature-Method必须为`HmacSHA256`；

API-Signature-Version必须为`1`；

API-Timestamp为当前时间戳，单位为毫秒整数，不支持小数，误差不得超过1分钟；

API-Unique-ID为可选，如果提供，则客户端需要提供一个唯一字符串标识，参考[API Unique Id](unique-id)；

带Body的请求，需要先序列化为JSON字符串，然后把Body列入计算签名。不要对一个对象使用两次序列化，因为某些语言的实现可能导致两次序列化的JSON不一样，例如，`{"a":true, "b":1}`和`{"b":1, "a":true}`，两者JSON内容一致但字符串不同，将导致验证签名失败。

### SDK

我们提供如下的SDK封装了签名算法样例，帮助开发者快速调用API：

* Java SDK [github](https://github.com/876ex-pub/signature-demo/blob/master/ApiClient.java)
* Python SDK [github](https://github.com/876ex-pub/signature-demo/blob/master/876ex.py)
* JavaScript/Node SDK [github](https://github.com/876ex-pub/signature-demo/blob/master/876ex.js)
