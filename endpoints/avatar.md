# 头像

1.可以直接获取用户不同尺寸的头像 2.可以在 HTML 中内联加载用户的头像

## 显示用户的头像

```
GET a.wunderlist.com/api/v1/avatar
```

名称       | 类型      | 备注
:------- | :------ | :---------------------------------------------------------------------------------------------
user_id  | integer | **必须**
size     | integer | **可选的**, 可选值: 25, 28, 30, 32, 50, 54, 56, 60, 64, 108, 128, 135, 256, 270, 512 以及 original
fallback | boolean | **可选的**

### Response

```
302 Found
```

该请求将会根据 `user_id` 将您重定向到请求给定的用户的头像的网址, 如果没有头像, 则会重定向到默认的头像图像。

**Info:** 该请求不需要你发送 `X-Access-Token`

**Optional**
如果您在请求的参数里包含了可选项 `size`,  它将重定向到具有给定 `size` 的头像。

你可以通过把请求的可选项 `fallback` 设置为 `false` 从而禁止重定向至 API 返回的头像。 如果没有给定的 `user_id` 所对应的自定义头像, 将会返回一个 `204 No Content` 和一个空体。(If there is no custom avatar uploaded for the given `user_id` it will respond with a `204 No Content` and an empty body.)
