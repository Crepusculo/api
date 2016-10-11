## 复原和同步

Wunderlist API 的每个实体拥有一个只读的 `revision` 属性。`revision` 属性是一个每当实体或其任意子成员在响应后被更新就会改变的整型。当任务的标题改变的时候，任务的复原属性也会更新，同时该任务所有的爹娘节点包括列表和根实体的复原属性也会一起改变。

### 更新实体

In order to guarantee that updates to Wunderlist entities are correctly executed and kept in sync across clients, any changes to an entity through the API must be accompanied by the `revision` property. The server uses this property to ensure that the client has the most up-to-date version of the entity. If a client makes a request with an out-of-date `revision` property, the request will fail, indicating that the client needs to fetch the entity’s current state and try again.

If an update request fails, you must fetch the current version of the entity, look for attributes that conflict with your local state e.g. content on a note, and do some sort of local conflict resolution before replaying your changes to the API with the current revision.

### Matryoshka / Russian Doll Hierarchy

All of the elements in Wunderlist’s data model are stored in a tree with revisions bubbling up the tree when data changes. This tree is:

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

You can completely synchronize a local copy of the Wunderlist data model with the Wunderlist API by checking the root `revision` property, descending if necessary, and repeating the process for each leaf in the tree.
你可以同步 `revision` 属性， Wunderlist API 完全同步本地拷贝

When a russian doll sync occurs on a client, the following rules apply:

Fetched revision values and data should not be committed to local models and persistence layers unless child resources are successfully fetched. This means you should not update the child-revision of the parent until all child data has been successfully fetched. E.g. you should not apply list data and revision changes unless all tasks were fetched successfully, etc.

Deleted items can be found by comparing your local data to the data retrieved during a russian doll sync and comparing for missing ids. However, since tasks may be moved to another list, you should mark a task as missing and only delete it if it is not present in any lists when the russian doll sync has completed successfully. This pattern can be extended to any model type that is “moveable”.
