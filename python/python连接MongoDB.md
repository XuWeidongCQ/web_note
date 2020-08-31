# 一 、使用模块

```python
#下载
pip install pymongo
conda install pymongo
```

# 二、连接数据库 集合

```python
import pymongo

#pymongo的所有属性和方法
['ALL', 'ASCENDING', 'CursorType', 'DESCENDING', 'DeleteMany', 'DeleteOne', 'GEO2D', 'GEOHAYSTACK', 'GEOSPHERE', 'HASHED', 'IndexModel', 'InsertOne', 'MAX_SUPPORTED_WIRE_VERSION', 'MIN_SUPPORTED_WIRE_VERSION', 'MongoClient', 'MongoReplicaSetClient', 'OFF', 'ReadPreference', 'ReplaceOne', 'ReturnDocument', 'SLOW_ONLY', 'TEXT', 'UpdateMany', 'UpdateOne', 'WriteConcern', 'aggregation', 'auth', 'bulk', 'change_stream', 'client_options', 'client_session', 'collation', 'collection', 'command_cursor', 'common', 'compression_support', 'cursor', 'cursor_manager', 'database', 'driver_info', 'encryption_options', 'errors', 'get_version_string', 'has_c', 'helpers', 'ismaster', 'max_staleness_selectors', 'message', 'mongo_client', 'mongo_replica_set_client', 'monitor', 'monitoring', 'monotonic', 'network', 'operations', 'periodic_executor', 'pool', 'read_concern','read_preferences', 'response', 'results', 'saslprep', 'server', 'server_description', 'server_selectors', 'server_type', 'settings', 'son_manipulator', 'srv_resolver', 'ssl_match_hostname', 'ssl_support', 'thread_util', 'topology', 'topology_description', 'uri_parser', 'version', 'version_tuple', 'write_concern']


#1.输入正确的url,连接到MongoDB
dbClient = pymongo.MongoClient("mongodb://localhost:27017/") 
dbClient = pymongo.MongoClient('localhost',27017)
#dbClient的所有属性和方法
['HOST', 'PORT', 'address', 'arbiters', 'close', 'close_cursor', 'codec_options', 'database_names','drop_database', 'event_listeners', 'fsync', 'get_database', 'get_default_database', 'is_locked', 'is_mongos', 'is_primary', 'kill_cursors', 'list_database_names', 'list_databases', 'local_threshold_ms', 'max_bson_size', 'max_idle_time_ms', 'max_message_size', 'max_pool_size', 'max_write_batch_size', 'min_pool_size', 'next', 'nodes', 'primary', 'read_concern', 'read_preference', 'retry_reads', 'retry_writes', 'secondaries', 'server_info', 'server_selection_timeout', 'set_cursor_manager', 'start_session', 'unlock', 'watch', 'write_concern']


#2.找到具体的数据库
mydb = myclient[database_name]
#mydb的所有属性和方法
['add_son_manipulator', 'add_user', 'aggregate', 'authenticate', 'client', 'codec_options', 'collection_names', 'command', 'create_collection', 'current_op', 'dereference', 'drop_collection', 'error', 'eval', 'get_collection', 'incoming_copying_manipulators', 'incoming_manipulators', 'last_status', 'list_collection_names', 'list_collections', 'logout', 'name', 'next', 'outgoing_copying_manipulators', 'outgoing_manipulators', 'previous_error', 'profiling_info', 'profiling_level', 'read_concern', 'read_preference', 'remove_user', 'reset_error_history', 'set_profiling_level', 'system_js', 'validate_collection', 'watch', 'with_options', 'write_concern']

#3.连接到具体的集合
mycol = mydb[collection_name]
#mycol的所有属性和方法[重要!!!!!]
[
'aggregate', 'aggregate_raw_batches', 'bulk_write', 'codec_options', 'count', 'count_documents', 'create_index', 'create_indexes', 'database', 'delete_many', 'delete_one', 'distinct', 'drop', 'drop_index', 'drop_indexes', 'ensure_index', 'estimated_document_count', 'find', 'find_and_modify', 'find_one', 'find_one_and_delete', 'find_one_and_replace', 'find_one_and_update', 'find_raw_batches', 'full_name', 'group', 'index_information', 'initialize_ordered_bulk_op', 'initialize_unordered_bulk_op', 'inline_map_reduce', 'insert', 'insert_many', 'insert_one', 'list_indexes', 'map_reduce', 'name', 'next', 'options', 'parallel_scan', 'read_concern', 'read_preference', 'reindex', 'remove', 'rename', 'replace_one', 'save', 'update', 'update_many', 'update_one', 'watch', 'with_options', 'write_concern']
```

# 三、插入记录

```python
dict = { "name": "Bill", "address": "Highway 37" }

#插入一条记录
mycol.insert_one(dict) #返回一个InsertOneResult 对象

#插入多条记录
mylist = [
  { "name": "Amy", "address": "Apple st 652"},
  { "name": "Hannah", "address": "Mountain 21"},
  { "name": "Michael", "address": "Valley 345"},
  { "name": "Sandy", "address": "Ocean blvd 2"},
  { "name": "Betty", "address": "Green Grass 1"},
  { "name": "Richard", "address": "Sky st 331"},
  { "name": "Susan", "address": "One way 98"},
  { "name": "Vicky", "address": "Yellow Garden 2"},
  { "name": "Ben", "address": "Park Lane 38"},
  { "name": "William", "address": "Central st 954"},
  { "name": "Chuck", "address": "Main Road 989"},
  { "name": "Viola", "address": "Sideway 1633"}
]
x = mycol.insert_many(mylist) #返回一个InsertManyResult 对象
```

# 四、查找记录

```python
#查找一条记录
mycol.find_one() #返回字典

#查找多条记录
mycol.find(query,skip) #返回pymongo.cursor.Cursor 可迭代对象

#限定查找记录的条数
mycol.find(query,skip).limit(length)
```

```python
# find()方法的第一个参数是一个query对象，用来过滤查询的记录 下面是几个例子

myquery = { "address": { "$gt": "S" } } #查询的记录的address字段的值的开头字母必须大于S
myquery = { "address": "Park Lane 38" } #查询的记录的address字段的值的开头字母必须等于Park Lane 38
myquery = { "address": { "$regex": "^S" } } #查询的记录的address字段的值的开头字母必须以S开头

# find()方法的第二个参数是一个skip对象，用来提示要显示的记录的字段

skip = {'_id':0} #不显示记录中的_id字段(不能在里面同时写0 和 1)
```

## 4.1 条件条件

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20200810174231659.png" alt="image-20200810174231659" style="zoom:67%;" />

![image-20200810174312722](../../../AppData/Roaming/Typora/typora-user-images/image-20200810174312722.png)

## 4.2 多条件查询

![image-20200810174502326](../../../AppData/Roaming/Typora/typora-user-images/image-20200810174502326.png)

# 五、删除记录

```python
myquery = { "address": "Mountain 21" }

#删除一条记录
mycol.delete_one(query) #删除一条记录的address为Mountain 21的记录

#删除多条记录
mycol.delete_many(query) #删除所有记录的address为Mountain 21的记录

#删除集合中所有记录
mycol.delete_many({})
```

# 六、修改记录

```python

#修改一条记录
myquery = { "address": "Valley 345" }
newvalues = { "$set": { "address": "Canyon 123" } } #$set为修改器
mycol.update_one(myquery, newvalues) #myquery表示要进行修改的记录 newvalues表示新的值

#修改多条记录
mycol.update_many(myquery, newvalues)
```

## 6.1 修改器说明

| 修改器 | 举例                                          | 说明                                                         |
| ------ | --------------------------------------------- | ------------------------------------------------------------ |
| $inc   | {'$inc':{'size':1}}                           | 将记录中中size字段增加1(只能针对数字型的字段)                |
| $set   | {'$set':{'size':1}} {'$set':{'size.width':1}} | 将记录中中size字段(size中的width字段)变为1,用来指定一个键并更新键值，若键不存在并创建 |
| $unset | {'$unset':{'size':1}}                         | 删除记录中的size字段                                         |
| $push  |                                               | 向记录中的值为数组的末尾追加一个元素                         |
| $pop   |                                               |                                                              |
| $pull  |                                               |                                                              |



# 七、删除集合

```python
mycol.drop() #删除整个mycol集合，成功返回true,如果集合不存在就返回false
```

