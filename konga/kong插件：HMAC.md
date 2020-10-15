### HMAC
* 介绍与原理：HMAC(Hashing for Message Authentication)，哈希信息验证码，主要功能是保证数据的完整。客户端把请求头和请求体通过散列/哈希算法算出一个摘要，并把算法和内容以及摘要传送给服务端，服务端按照这个算法也算一遍，并和摘要对比。如果一样就认为内容是完整的，如果不一样就认为内容被篡改了并返回一个错误消息。

* 兼容的协议：`http`, `https`, `grpc`, `grpcs`

* 配置方法：可以通过 `Kong Admin API`、`配置文件`或者`图形化界面konga`配置HMAC插件。

    * 使用Kong Admin API进行配置<br>

        1，在Service中使用该插件，命令行中输入下面的请求：<br>
        注：\<admin-hostname\>为kong网关的host，如果是本地配置的kong，\<admin-hostname\> = localhost；\<service\>可以是Service的id或者名字。

        ```bash
        curl -X POST http://<admin-hostname>:8001/services/<service>/plugins \
        --data "name=hmac-auth" 
        ```

        2，在Route中使用该插件，命令行中输入下面的请求：<br>
        注：\<route\>可以是Route的id或者名字。
        ```bash
        curl -X POST http://<admin-hostname>:8001/routes/<route>/plugins \
        --data "name=hmac-auth" 
        ```
   
        3，全局使用该插件，命令行中输入下面的请求：<br>
        注：全局是指该插件不关联于特定的某个Service、Route、Consumer，并且对任意的请求都会生效。
        ```bash
        curl -X POST http://<admin-hostname>:8001/plugins/ \
        --data "name=hmac-auth" 
        ```

    * 插件配置的请求参数

    |参数           |描述                                               |
    |---            |---                                                |
    |`name`         |配置的插件的名字。本例中名字为`hmac-auth`            |
    |`service.id`   |该插件关联的Service的id。使用name无效                 |
    |`route.id`     |该插件关联的Route的id                                |
    |`enabled`<br>默认值：`true`|是否使用该插件                           |
    |`config.hide_credentials`<br>默认值：`false`|一个可选择的bool值，用来告诉该插件是否隐藏来自upstream service的证书。如果值为`true`，该插件将会在代理之前从请求（Authorization header）中剥离凭证   |
    |`config.clock_skew`<br>默认值：`300`|以秒为单位的数值；发送的请求中带的date与当前时间的差值大于clock_skew时，将会返回错误的信息。可以防止replay attacks<br>客户端需要在请求头中加入有效的date(GMT格式)。|
    |`config.validate_request_body`<br>默认值：`false`|是否使请求体生效。如果该值为true，插件将会计算请求体的`SHA-256`HMAC摘要，并将其与请求头的摘要匹配。<br>请求头的摘要格式如：Digest: SHA-256=base64(sha256(\<body\>))|
    |`config.enforce_headers`|一个客户端在创造HTTP签名时至少需要用到的headers列表。默认情况下该插件不会强制要求使用哪个headers列表，但是建议headers列表需要包括：`request-line`, `host`, 和 `date`|
    |`config.algorithms`<br>默认值：`hmac-sha1`,`hmac-sha256`,`hmac-sha384`,`hmac-sha512`|用户想要该插件支持的HMAC摘要算法|

    * 请求示例
    ```bash
    curl -X POST http://<admin-hostname>:8001/plugins/ \
    --data "name=hmac-auth" \
    --data "service.id=<service_id>" \
    --data "route.id=<route_id>" \
    --data "enabled=true" \
    --data "config.hide_credentials=false" \
    --data "config.clock_skew=300" \
    --data "config.validate_request_body=false" \
    --data "config.enforce_headers=request-line" \
    --data "config.algorithms=hmac-sha1" \
    --data "config.algorithms=hmac-sha512"
    ```

* 使用方法：为了使用该插件，我们需要先创建一个至少关联了一个证书的消费者。<br>
注：因为HMAC的签名是由客户端生成，我们需要保证在该插件运行之前kong网关没有更新或者删除任何一个请求参数。
    * 1，创建一个消费者<br>
        在DB模式下，命令行中输入下列请求：
        ```bash
        curl -d "username=user123&custom_id=SOME_CUSTOM_ID" http://<admin-hostname>:8001/consumers/
        ```
        请求参数如下：
        注：username和custom_id参数至少需要明确指定一个。
    
        |参数|描述|
        |---|---|
        |`username`|消费者的名字。|
        |`custom_id`|消费者的id。|

    * 2，创建一个证书<br>
        在DB模式下，命令行输入下列请求：
        ```bash
        curl -X POST http://<admin-hostname>:8001/consumers/{consumer}/hmac-auth \
        --data "username=bob" \
        --data "secret=secret456"
        ```
        请求参数如下：
        |参数|描述|
        |---|---|
        |`{consumer}`|与该证书相关联的消费者的`id`或者`username`|
        |`username`|用于HMAC签名认证的username|
        |`secret`|用于HMAC签名认证的密钥。如果该参数未被提供，kong将自动生成一个secret值并作为结果返回。|

    * 3，签名认证方案<br>
        客户端应该发送一个`Authorization` 或者 `Proxy-Authorization`的请求头，该请求头需要包含下列参数：
        ```bash
        credentials := "hmac" params
        params := keyId "," algorithm ", " headers ", " signature
        keyId := "username" "=" plain-string
        algorithm := "algorithm" "=" DQUOTE (hmac-sha1|hmac-sha256|hmac-sha384|hmac-sha512) DQUOTE
        headers := "headers" "=" plain-string
        signature := "signature" "=" plain-string
        plain-string   = DQUOTE *( %x20-21 / %x23-5B / %x5D-7E ) DQUOTE
        ```
        请求参数如下：
        |参数|描述|
        |---|---|
        |`username`|证书的用户名|
        |`algorithm`|用来创建签名的数字签名算法|
        |`headers`|HTTP请求的名字的列表，由一个空格分隔|
        |`signature`|由客户端生成的用base64编码的数字签名|

    * 签名字符串（Signature String）的构造<br>
    为了生成用密钥签名的字符串，客户端必须按照每个http请求头出现的顺序来获取它们的值。<br>
        * 1，如果请求头名字不是request-line，那么就加上小写的请求头名字以及ASCII码的`:`，以及ASCII码的空格` `。
        * 2，如果请求头名字是request-line，那么就加上HTTP的请求行（ASCII码形式)，否则加上请求头的值。
        * 3，如果该值不是最后一个值，需要加上ASCII码的换行`\n`。

    * 请求体验证<br>
    用户可以通过设置config.validate_request_body的值为true来开启请求体验证。HMAC-auth插件将会计算请求体的SHA-256 HMAC摘要，并将其与请求头中的摘要进行匹配。摘要的算法如下：
        ```
        Digest: SHA-256=base64(sha256(<body>))
        ```


* HMAC插件使用实例
    * 1，创建一个Service
        ```bash
        $ curl -i -X POST http://localhost:8001/services \
        -d "name=example-service" \
        -d "url=http://example.com"
        ```

    * 2，创建一个Route
        ```bash
        $ curl -i -f -X POST http://localhost:8001/services/example-service/routes \
        -d "paths[]=/"
        ```

    * 3，使HMAC插件在指定的Service上生效
        ```bash
        $ curl -i -X POST http://localhost:8001/services/example-service/plugins \
        -d "name=hmac-auth" \
        -d "config.enforce_headers=date" \
        -d "config.enforce_headers=request-line" \
        -d "config.algorithms=hmac-sha1" \
        -d "config.algorithms=hmac-sha256"
        ```

    * 4，添加一个Consumer
        ```bash
        $ curl -i -X POST http://localhost:8001/consumers/ \
        -d "username=alice"
        ```

    * 5，为指定的Consumer添加一个证书
        ```bash
        $ curl -i -X POST http://localhost:8001/consumers/alice/hmac-auth \
        -d "username=alice123" \
        -d "secret=secret"
        ```

    * 6，发送一个已认证的请求<br>
        注：这个请求的请求时间为Thu, 22 Jun 2017 17:15:21，为了通过认证，建议将HMAC-auth插件的clock skew设置大一点，比如999999999999999
        ```bash
        $ curl -i -X GET http://localhost:8000/requests \
        -H "Host: hmac.com" \
        -H "Date: Thu, 22 Jun 2017 17:15:21 GMT" \
        -H 'Authorization: hmac username="alice123", algorithm="hmac-sha256", headers="date request-line", signature="ujWCGHeec9Xd6UD2zlyxiNMCiXnDOWeVFMu5VeRUxtw="'
        ```
        上面的请求中signature的计算方法如下：<br>
        ```????????????????????????????计算方法碰到问题```
        ```
        signing_string="date: Thu, 22 Jun 2017 17:15:21 GMT\nGET /requests HTTP/1.1"
        digest=HMAC-SHA256(<signing_string>, "secret")
        base64_digest=base64(<digest>)
        signature = <base64_digest>
        ```

    * 7，设置config.validate_request_body为true以进行请求体的验证
        ```bash
        $ curl -i -X PATCH http://localhost:8001/plugins/{plugin-id} \
        -d "config.validate_request_body=true"
        ```
        在进行请求时，客户端需要计算请求体的摘要并将它放到请求头中，插件在收到该请求时会再次计算请求体的摘要并与请求头传入的摘要进行对比。如果两者相同则说明请求体没有被篡改。
        ```bash
        $ curl -i -X GET http://localhost:8000/requests \
        -H "Host: hmac.com" \
        -H "Date: Thu, 22 Jun 2017 21:12:36 GMT" \
        -H "Digest: SHA-256=SBH7QEtqnYUpEcIhDbmStNd1MxtHg2+feBfWc1105MA=" \
        -H 'Authorization: hmac username="alice123", algorithm="hmac-sha256", headers="date request-line digest", signature="gaweQbATuaGmLrUr3HE0DzU1keWGCt3H96M28sSHTG8="' \
        -d "A small body"
        ```


参考文档
https://docs.konghq.com/hub/kong-inc/hmac-auth/

