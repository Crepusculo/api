## 授权

奇妙清单API 不直接使用用户密码，而是使用[OAuth2](http://oauth.net/2/)授权其它外部应用访问用户的奇妙清单账号的请求。

开发者在必须在使用这些API之前[注册他们的应用](https://developer.wunderlist.com/apps/new)。App的注册需要一个用户ID和一个用户密码，之后你就可以在奇妙清单的用户在你的应用里授权后，通过 token 访问他们的账号信息了。

After a user has authorized your application and you have an access token, you can use it in Wunderlist API requests by setting the `X-Client-ID` and `X-Access-Token` HTTP request headers.
在用户授权，应用获得 token 后，你可以通过设置 `X-Client-Id` 和 `X-Access-Token` HTTP request headers 使用奇妙清单API。

    X-Access-Token: OAUTH-TOKEN X-Client-ID: CLIENT-ID

举个例子，你可以这样设置你的 curl 的授权头文件：

    curl -H "X-Access-Token: OAUTH-TOKEN" -H "X-Client-ID: CLIENT-ID" https://a.wunderlist.com/api/v1/user

<div class="p2 rounded border border-red bg-transparent-red">
	<strong class="bold">注意!</strong>
	Please note that setting the above mentioned headers is required to access protected resources on the Wunderlist API. OAuth2 is only used for obtaining user tokens.
</div>

### Web Server Application Integration
Web服务应用注册

To integrate your third-party web server application with Wunderlist, use the following flow:


#### 1. Redirect users to request Wunderlist access

    https://www.wunderlist.com/oauth/authorize?client_id=ID&redirect_uri=URL&state=RANDOM

#### 参数

名称 | 类型 | 描述
-----|------|--------------
`client_id`|`string` | **Required**. Client ID 是你在[注册应用](https://developer.wunderlist.com/apps/new)时收到的那个。
`redirect_uri`|`string` | **Required**. The URL in your app where users will be sent after authorization. See details below about [redirect urls](#redirect-urls).
`state`|`string` | **Required**. An unguessable random string. It is used to protect against cross-site request forgery attacks.

#### 2. Wunderlist redirects back to your site

If the user accepts your request, Wunderlist will redirect to your `redirect_uri`
with a temporary code in a `code` parameter as well as the state you provided in
the previous step in a `state` parameter. If the states don't match, the request
has been created by a third party and the process should be aborted.

Exchange `code` for an access token:

    POST https://www.wunderlist.com/oauth/access_token

### JSON Data

Name | Type | Description
-----|------|---------------
`client_id`|`string` | **Required**. The client ID you received from Wunderlist when you [registered](https://developer.wunderlist.com/apps/new).
`client_secret`|`string` | **Required**. The client secret you received from Wunderlist when you [registered](https://developer.wunderlist.com/apps/new).
`code`|`string` | **Required**. The code you received as a response to [Step 1](#1-redirect-users-to-request-wunderlist-access).

### Response

A successful response will take the following form:

    {
      "access_token": "976d16a81ccf621a654fcc23193b09498b220e89eb72ced3"
    }

### Libraries

If you are developing a Rails app and are using omniauth, you can use the [omniauth-wunderlist strategy](https://rubygems.org/gems/omniauth-wunderlist) to managing redirects and code exchange for you.
