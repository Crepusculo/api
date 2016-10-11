## 格式

API 的每个请求和响应都通过 [JSON](http://www.json.org)-JavaScript Object Notation 来传输。JSON 易于使用而且有广泛可用的实现。本文档中你所见到的 POST, PATCH, PUT 的请求，都必须通过 JSON 来传输你的数据。

For example creating a task:
例：创建一个新的任务

    POST a.wunderlist.com/api/v1/tasks

    {
      "list_id": 12345,
      "title": "Hallo",
      "assignee_id": 123,
      "completed": true,
      "due_date": "2013-08-30",
      "starred": false
    }

### Content-Type

所有 POST, PATCH 或 PUT 的 JSON 请求必须设置一个含有 `application/json` 值的 `Content-Type` 头。未设置头文件或头文件值错误将会返回error。

### 编码

所有请求必须以 UTF-8 编码。所有响应的返回形式为 UTF-8。

### Date and Time Format

Wunderlist API 中所有的时间和日期都被格式化为 ISO-8601 字符串，所有的时间都以 UTC 时区时间提供。用例：`2013-10-09T23:34:11Z`

### Error Responses

错误响应将会以一个包含 `type`，`translation_key` 以及 `message` 参数的字典 `error`返回。

#### 401 Unauthorized 未授权

    {
      "error": {
        "type": "unauthorized",
        "translation_key": "api_error_unauthorized",
        "message": "You are not authorized."
      }
    }

#### 404 Not Found 未知请求

    {
      "error": {
        "type": "not_found",
        "translation_key": "api_error_not_found",
        "message": "The resource you requested could not be found."
      }
    }

#### 400 Bad Request 错误请求

一部分错误将会返回附加的参数。特别地，当发生 `message` 为 `Missing parameter` 的错误时，遗失的参数会和指示问题的信息一起被提供。

    {
      "error": {
        "type": "missing_parameter",
        "translation_key": "api_error_missing_params",
        "message": "Missing parameter.",
        "list_id": ["required", "can't be blank"]
      }
    }
