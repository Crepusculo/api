## 授权

Wunderlist API 不直接使用用户密码，而是允许其它应用通过[OAuth2](http://oauth.net/2/)请求授权访问用户的奇妙清单账户。

开发者在必须在使用这些API之前[注册他们的应用](https://developer.wunderlist.com/apps/new)。注册会给你的应用分配唯一的客户端ID和密码，之后就可以在奇妙清单的用户在你的应用里授权后，通过 token 访问他们的账户信息了。

在用户授权给你的应用，应用获得 token 后，你就可以使用该 token 来通过设置 `X-Client-Id` 和 `X-Access-Token` HTTP request headers 使用奇妙清单API。

    X-Access-Token: OAUTH-TOKEN X-Client-ID: CLIENT-ID

举个例子，你可以这样设置你的 curl 的授权头文件：

    curl -H "X-Access-Token: OAUTH-TOKEN" -H "X-Client-ID: CLIENT-ID" https://a.wunderlist.com/api/v1/user

<div class="p2 rounded border border-red bg-transparent-red">
	<strong class="bold">注意!</strong>
    需要设置上述头文件才能访问 WunderList API 上受保护的资源，OAuth2 仅用于获取用户 tokens。
    请注意，
</div>

### Web Server 应用集成

使用以下流程将第三方 Web 服务器应用程序与 Wunderlist 集成：

#### 1. 重定向用户访问请求至 Wunderlist

    https://www.wunderlist.com/oauth/authorize?client_id=ID&redirect_uri=URL&state=RANDOM

#### 参数

名称 | 类型 | 描述
-----|------|--------------
`client_id`|`string` | **Required**. 你在[注册应用](https://developer.wunderlist.com/apps/new)时收到的Client ID。
`redirect_uri`|`string` | **Required**. 授权后用户将会被重定向的网址。详见[重定向网址](#redirect-urls).
`state`|`string` | **Required**. 一串猜你妈嗨的随机字符串，用于防止跨站请求伪造(Cross-site request forgery, CSRF)攻击。

#### 2. Wunderlist 重定向返回你指定的网址

如果用户接受了您的 request，Wunderlist 将会重定向至你设置的 `redirect_uri`，其中包含在 `code` 中返回一个临时代码以及你在之前步骤中提供的 `state` 状态。
如果这个状态不匹配，那么这个请求已被第三方创建，且当前进程应当被中止。

用于访问 token 的交换 `code`：

    POST https://www.wunderlist.com/oauth/access_token

### JSON 数据

名称 | 类型 | 描述
-----|------|---------------
`client_id`|`string` | **Required**. 你在[注册应用](https://developer.wunderlist.com/apps/new)时收到的Client ID。
`client_secret`|`string` | **Required**. 你在[注册应用](https://developer.wunderlist.com/apps/new)时收到的Client secret。
`code`|`string` | **Required**. 你在[第一步](#1-重定向用户访问请求至-Wunderlist)中收到的 response 的代码。

### Response

成功后的响应将采取下面的形式：

    {
      "access_token": "976d16a81ccf621a654fcc23193b09498b220e89eb72ced3"
    }

### 库

如果你在开发一个 Rails 应用并且正在使用 omniauth，你可以使用 [omniauth-wunderlist strategy](https://rubygems.org/gems/omniauth-wunderlist) 来管理重定向和代码交换。
