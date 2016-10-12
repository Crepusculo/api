## 修订和同步

Wunderlist API 的每个实体拥有一个只读的 `revision` 属性。`revision` 属性是一个每当实体或其任意子成员在响应后被更新就会改变的整型。当任务的标题改变的时候，任务的修订（revision）属性也会更新，同时该任务所有的爹娘节点包括列表和根实体的复原属性也会一起改变。

### 更新实体

In order to guarantee that updates to Wunderlist entities are correctly executed and kept in sync across clients, any changes to an entity through the API must be accompanied by the `revision` property. The server uses this property to ensure that the client has the most up-to-date version of the entity. If a client makes a request with an out-of-date `revision` property, the request will fail, indicating that the client needs to fetch the entity’s current state and try again.

If an update request fails, you must fetch the current version of the entity, look for attributes that conflict with your local state e.g. content on a note, and do some sort of local conflict resolution before replaying your changes to the API with the current revision.

### 继承之俄罗斯套娃

Wunderlist 数据模型的所有元素存储在一个树中，当数据发生改变时，修订后版本会往上冒泡。

- root
    * list
        - task
            * file
            * task_comment
            * note
            * subtask
            * subtask_positions
        - task_positions
        - membership
    * list_positions
    * user
        - setting
        - reminder
        - avatar

### 同步

若必要的话，你可以通过 Wunderlist API 检查根 `revision` 属性，降序地重复遍历每一个树的叶节点，从而完全同步本地拷贝。

当一个客户端发生一个俄罗斯套娃式的同步时，其遵循如下规则：


This means you should not update the child-revision of the parent until all child data has been successfully fetched. E.g. you should not apply list data and revision changes unless all tasks were fetched successfully, etc.

除非成功获取了子资源，否则获取的修订值和数据不应被提交到本地模型和持久化层。也就是说，我们在一个父数据的所有子数据都成功获取之前，不应该更新其 child-revision。举个例子，除非所有任务已被成功获取，否则我们不应该应用列表数据和修订的更改。 


Deleted items can be found by comparing your local data to the data retrieved during a russian doll sync and comparing for missing ids. However, since tasks may be moved to another list, you should mark a task as missing and only delete it if it is not present in any lists when the russian doll sync has completed successfully. This pattern can be extended to any model type that is “moveable”.
