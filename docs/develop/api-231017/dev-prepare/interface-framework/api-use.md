
# API 调用｜鉴权

> QQ BOT 服务端开放的 openapi 接口，均使用 https 方式进行调用，通过 access token 机制实现对 openapi 接口调用的鉴权。

## 获取接口凭证

- **请求**

| 基本 |
| --- |
| HTTP URL | https://bots.qq.com/app/getAppAccessToken |
| HTTP Method | POST |
| 接口频率限制 |      |

- **请求参数**

| **属性** | **类型** | **必填** | **说明** |
| --- | --- | --- | --- |
| appId | string | 是 | 在开放平台管理端上获得。 |
| clientSecret | string | 是 | 在开放平台管理端上获得。 |

- **返回参数**

| **属性** | **类型** | **说明** |
| --- | --- | --- |
| access\_token | string | 获取到的凭证。 |
| expires\_in | number | 凭证有效时间，单位：秒。目前是7200秒之内的值。 |

- **错误码**

| **错误码** | **错误码取值** | **解决方案** |
| --- | --- | --- |
| 0 | ok | |

- **调用示例**

```
curl --location 'https://bots.qq.com/app/getAppAccessToken' \
--header 'Content-Type: application/json' \
--data '{
"appId": "APPID",
"clientSecret": "CLIENTSECRET"
}'
```

- **返回示例**
```
{
"access\_token": "ACCESS\_TOKEN",
"expires\_in": "7200"
}
```

- **其他说明**

目前 access\_token 生命周期默认 7200 秒，每次请求不会刷新新的 access\_token，开发者需要在过期后自行刷新 access\_token，保证调用链路权限正常。

在上一个 access\_token 接近过期时间 60 秒内，获取 access\_token 时，会获得一个新的 access\_token，老的 access\_token 在这个60秒内仍然有效。

## 接口调用

在每次调用 https 的 openapi 开放接口请求的时候，需要在 header 内引入 access\_token 进行调用权限验证。

**统一地址**

> https://api.sgroup.qq.com


**请求头**

| 名称 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| Authorization | string | 是 | 格式值："QQBot ACCESS\_TOKEN" |
| X-Union-Appid | string | 是 | 格式值："BOT\_APPID", 机器人 appid |

**事例**
```
'headers': {
'Authorization': "QQBot {ACCESS\_TOKEN}",
'X-Union-Appid': "{BOT\_APPID}",
}
```

## 调用权限错误码

与 access token 权限有关的错误码。