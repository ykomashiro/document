# 1. MongoDB

- [1. MongoDB](#1-mongodb)
    - [1.1. 连接MongoDB](#11-连接mongodb)
    - [1.2. 指定数据库](#12-指定数据库)
    - [1.3. 指定集合](#13-指定集合)
    - [1.4. 插入数据](#14-插入数据)
    - [1.5. 查询](#15-查询)
    - [1.6. 计数](#16-计数)
    - [1.7. 排序](#17-排序)
    - [1.8. 偏移](#18-偏移)
    - [1.9. 更新](#19-更新)
    - [1.10. 删除](#110-删除)
    - [1.11. 其他操作](#111-其他操作)

## 1.1. 连接MongoDB

使用PyMongo库里面的 `MongoClient` 连接MongoDB。一般来说，需要传入MongoDB的IP及端口即可，其中第一个参数为地址host，第二个参数为端口port（如果该参数为空，默认是27017）：

```python
import pymongo
client = pymongo.MongoClient(host='localhost', port=27017)
```

另外，MongoClient的第一个参数host还可以直接传入MongoDB的连接字符串，它以 `mongodb` 开头，例如：

```python
client = MongoClient('mongodb://localhost:27017/')
```

## 1.2. 指定数据库

`db = client.test`

`db = client['test']`

## 1.3. 指定集合

MongoDB的每个数据库又包含许多集合（collection），它们类似于关系型数据库中的表。

`collection = db.students`

`collection = db['students']`

## 1.4. 插入数据

每一条数据以字典的形式表示：

```python
student = {
    'id': '20181004',
    'name': 'Mashiro',
    'age': 20,
    'gender': 'male'
}
```

指定信息后，直接调用 `collection` 的 `insert()` 方法即可插入数据。

```python
result = collection.insert(student)
```

如果想要插入多条数据，只需要以列表的形式传递：

```python
result = collection.insert([student1, student2])
```

在新版本中，官方已经不推荐使用 `insert()` 方法了。官方推荐使用 `insert_one()` 和 `insert_many()` 方法来分别插入单条记录和多条记录，示例如下：

```python
result = collection.insert_one(student)
result = collection.insert_many([student1, student2])
```

## 1.5. 查询

插入数据后，我们可以利用 `find_one()` 或 `find()` 方法进行查询，其中 `find_one()` 查询得到的是单个结果，`find()` 则返回一个生成器对象。示例如下：

```python
result = collection.find_one({'name': 'Mike'})
print(type(result))
print(result)
```

查询结果如下，它的返回结果是字典类型。

```python
<class 'dict'>
{'_id': ObjectId('5932a80115c2606a59e8a049'), 'id': '20170202', 'name': 'Mike', 'age': 21, 'gender': 'male'}
```

对于多条数据的查询，我们可以使用 `find()` 方法。

```python
results = collection.find({'age': 20})
print(results)
for result in results:
    print(result)
```

`find()` 返回结果是 `Cursor` 类型，它相当于一个生成器，我们需要遍历取到所有的结果，其中每个结果都是字典类型。

如果要查询年龄大于20的数据，则写法如下：

```python
results = collection.find({'age': {'$gt': 20}})
```

这里查询的条件键值已经不是单纯的数字了，而是一个字典，其键名为比较符号$gt

这里将比较符号归纳为下表。

symbol | mean           | example
-------|----------------|------------------------------
`$lt`  | less than      | `{'age': {'$lt': 20}}`
`$gt`  | great than     | `{'age': {'$gt': 20}}`
`$lte` | less or equal  | `{'age': {'$lte': 20}}`
`$gte` | great or equal | `{'age': {'$gte': 20}}`
`$ne`  | not equal      | `{'age': {'$ne': 20}}`
`$in`  | Within range   | `{'age': {'$in': [20, 23]}}`
`$nin` | not in         | `{'age': {'$nin': [20, 23]}}`

正则匹配查询
符号      | 含义           | 示例                                                | 示例含义
----------|--------------|-----------------------------------------------------|--------------------
`$regex`  | 匹配正则表达式 | `{'name': {'$regex': '^M.*'}}`                      | `name`以M开头
`$exists` | 属性是否存在   | `{'name': {'$exists': True}}`                       | name属性存在
`$type`   | 类型判断       | `{'age': {'$type': 'int'}}`                         | age的类型为int
`$mod`    | 数字模操作     | `{'age': {'$mod': [5, 0]}}`                         | 年龄模5余0
`$text`   | 文本查询       | `{'$text': {'$search': 'Mike'}}`                    | text类型的属性中包含Mike字符串
`$where`  | 高级条件查询   | `{'$where': 'obj.fans_count == obj.follows_count'}` | 自身粉丝数等于关注数

关于这些操作的更详细用法，可以在[MongoDB官方文档](https://docs.mongodb.com/manual/reference/operator/query/)找到：

## 1.6. 计数

要统计查询结果有多少条数据，可以调用`count()`方法。

```python
count = collection.find().count()
```

或者统计符合某个条件的数据：

```python
count = collection.find({'age': 20}).count()
```

## 1.7. 排序

排序时，直接调用`sort()`方法，并在其中传入排序的字段及升降序标志即可。示例如下：

```python
results = collection.find().sort('name', pymongo.ASCENDING)
print([result['name'] for result in results])
```

运行结果如下：

```python
['Harden', 'Jordan', 'Kevin', 'Mark', 'Mike']
```

如果要降序排列，可以传入`pymongo.DESCENDING`。

## 1.8. 偏移

在某些情况下，我们可能想只取某几个元素，这时可以利用`skip()`方法偏移几个位置，比如偏移2，就忽略前两个元素，得到第三个及以后的元素：

```python
results = collection.find().sort('name', pymongo.ASCENDING).skip(2)
print([result['name'] for result in results])
```

运行结果如下：

```python
['Kevin', 'Mark', 'Mike']
```

另外，还可以用`limit()`方法指定要取的结果个数，示例如下：

```python
results = collection.find().sort('name', pymongo.ASCENDING).skip(2).limit(2)
print([result['name'] for result in results])
```

运行结果如下：

```python
['Kevin', 'Mark']
```

值得注意的是，在数据库数量非常庞大的时候，如千万、亿级别，最好不要使用大的偏移量来查询数据，因为这样很可能导致内存溢出。此时可以使用类似如下操作来查询：

```python
from bson.objectid import ObjectId
collection.find({'_id': {'$gt': ObjectId('593278c815c2602678bb2b8d')}})
```

## 1.9. 更新

对于数据更新，我们可以使用 `update()` 方法，指定更新的条件和更新后的数据即可。例如：

```python
condition = {'name': 'Kevin'}
student = collection.find_one(condition)
student['age'] = 25
result = collection.update(condition, student)
print(result)
```

这里我们要更新 `name` 为Kevin的数据的年龄：首先指定查询条件，然后将数据查询出来，修改年龄后调用 `update()` 方法将原条件和修改后的数据传入。

另外，我们也可以使用 `$set` 操作符对数据进行更新，代码如下：

```python
result = collection.update(condition, {'$set': student})
```

这样可以只更新 `student` 字典内存在的字段。如果原先还有其他字段，则不会更新，也不会删除。而如果不用 `$set` 的话，则会把之前的数据全部用*student*字典替换；如果原本存在其他字段，则会被删除。

另外，`update()` 方法其实也是官方不推荐使用的方法。这里也分为 `update_one()` 方法和 `update_many()` 方法，用法更加严格，它们的第二个参数需要使用 `$` 类型操作符作为字典的键名，示例如下：

```python

condition = {'name': 'Kevin'}
student = collection.find_one(condition)
student['age'] = 26
result = collection.update_one(condition, {'$set': student})
print(result)
print(result.matched_count, result.modified_count)

```

这里调用了 `update_one()` 方法，第二个参数不能再直接传入修改后的字典，而是需要使用 `{'$set': student}` 这样的形式，其返回结果是 `UpdateResult` 类型。然后分别调用 `matched_count` 和 `modified_count` 属性，可以获得匹配的数据条数和影响的数据条数。

运行结果如下：

```python
<pymongo.results.UpdateResult object at 0x10d17b678>
1 0
```

## 1.10. 删除

删除操作比较简单，直接调用 `remove()` 方法指定删除的条件即可，此时符合条件的所有数据均会被删除。

```python
result = collection.remove({'name': 'Kevin'})
```

另外，这里依然存在两个新的推荐方法—— `delete_one()` 和 `delete_many()` 。示例如下：

```python
result = collection.delete_one({'name': 'Kevin'})
print(result)
print(result.deleted_count)
result = collection.delete_many({'age': {'$lt': 25}})
print(result.deleted_count)
```

运行结果如下：

```python
<pymongo.results.DeleteResult object at 0x10e6ba4c8>
1
4
```

`delete_one()` 即删除第一条符合条件的数据， `delete_many()` 即删除所有符合条件的数据。它们的返回结果都是 `DeleteResult` 类型，可以调用 `deleted_count` 属性获取删除的数据条数。

## 1.11. 其他操作

关于PyMongo的详细用法，可以参见[官方文档](http://api.mongodb.com/python/current/api/pymongo/collection.html)
