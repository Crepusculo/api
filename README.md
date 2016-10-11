## Wunderlist API Documentation

The Wunderlist API provides REST-based storage and synchronization of a user’s lists across multiple platforms and devices.

The primary things you’ll need to use it are an understanding of our data model, how we [version individual entities](concepts/revisions.md) in a user’s data, the [formats we use for transmission](concepts/formats.md), and a set of [OAuth credentials](concepts/authorization.md).

<!-- START_TOC -->

### Concepts

* [Authorization](concepts/authorization.md)
* [Formats](concepts/formats.md)
* [修订](concepts/revisions.md)

### Endpoints

* [头像](endpoints/avatar.md)
* [文件](endpoints/file.md)
* [文件预览](endpoints/file_preview.md)
* [文件夹](endpoints/folder.md)
* [列表](endpoints/list.md)
* [Membership](endpoints/membership.md)
* [笔记](endpoints/note.md)
* [位置](endpoints/positions.md)
* [Reminder](endpoints/reminder.md)
* [Root](endpoints/root.md)
* [子任务](endpoints/subtask.md)
* [任务](endpoints/task.md)
* [任务 评论](endpoints/task_comment.md)
* [上传](endpoints/upload.md)
* [用户](endpoints/user.md)
* [Webhooks](endpoints/webhooks.md)

### Tools

* [Wundlist.js SDK](tools/wunderlist.js.md)

<!-- END_TOC -->

### Support

The source for this documentation in at [GitHub ](https://github.com/wunderlist/api). If you have questions or need to report an issue, please [open an issue](https://github.com/wunderlist/api/issues).

### Code of Conduct
This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

### Compatibility

This API *will* evolve rapidly and future versions will add new endpoints and parameters. To provide a sane experience, the API is versioned. Within a version, the behavior and return values of the API will not change from currently documented behavior and return values. As well, the API and its data payloads are designed so that new parameters and return values can be added at any time.

To provide the best experience, a well-behaved Wunderlist API client must not send undocumented parameters as part of their request. These parameters could collide with a future version of the API. As well, a Wunderlist API client must accept and ignore any additional keys that it doesn’t handle in a response.


### Security

It should go without saying, but all requests must be made using a secure connection. We support a minimum of TLS 1.0. We also recommend that you use certificate pinning in clients that you ship and which may operate on untrusted public networks.
