# 头像

1.可以直接获取用户不同尺寸的头像 2.可以在 HTML 中内联加载用户的头像

## 显示用户的头像

```
GET a.wunderlist.com/api/v1/avatar
```

名称       | 类型      | 备注
:------- | :------ | :---------------------------------------------------------------------------------------------
user_id  | integer | **必须**
size     | integer | **可选的**, values: 25, 28, 30, 32, 50, 54, 56, 60, 64, 108, 128, 135, 256, 270, 512 and original
fallback | boolean | **必须**

### Response

```
302 Found
```

This request will redirect you to an url for the avatar of the given user based on the `user_id`, if there is no avatar it will redirect a default avatar image.

**Info:** You don't need to send an `X-Access-Token` for this request.

If you add the optional parameter `size` to the request it will redirect to an avatar with the given `size`.

By adding the optional parameter `fallback` with the value `false` to the request, you will not be redirected to our fallback avatars. If there is no custom avatar uploaded for the given `user_id` it will respond with a `204 No Content` and an empty body.
