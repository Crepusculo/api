## 文件预览

文件预览是指获得一个图像文件的缩略图。

### 获取某个特定文件的预览

预览端点 (Endpoint) 将会为给定的文件按需生成预览图像。目前这个端点**仅仅**对于图片生效。下面列出了一些支持生成预览的图片文件类型:
* `image/gif`
* `image/jpeg`
* `image/jpg`
* `image/pjpeg`
* `image/png`
* `image/svg+xml`
* `image/tiff`

#### 端点

    GET a.wunderlist.com/api/v1/previews

#### 参数

名称 | 类型 | 值 | 备注|
-----|------|--------|-------
file_id|integer||**必要的**
platform|string|`mac`, `web`, `windows`, `iphone`, `ipad`, `android`|**可选的**
size|string|`nonretina`, `retina`|**可选的**

#### Response

**成功时:**

    200 OK

```json
    {
      "url": "http://uploads-s3-wunderlist-production.com/preview-43ht43-file.md",
      "size": "420x420",
      "expires_at": "2013-08-02T11:58:55Z",
      "type": "preview"
    }
```

**失败时:**

    400 Bad Request

```json
    {
      "errors": ["bad request"]
    }
```

**Info:** 在失败时收到的 response 可能会和例子不一样。
