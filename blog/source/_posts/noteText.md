

---
title:笔记1
author: despair
avatar: http://127.0.0.1:8000/static/upload/touxiang.jpg
authorLink: despair1134
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 笔记1
date: 2020-4-21 19:40
comments: true
tags: 
 - web
 - 笔记
keywords: Sakura
description: 笔记1
---



<h2>上下文管理器（Context Manager）

```
上下文管理器是指在执行一段代码执行前，先执行一段代码用于一些预处理工作；执行这段代码之后再执行一段代码进行清理工作。比如打开文件进行读写，读写完之后需要将文件关闭。又比如在数据库中处理事务时，执行SQL前先连接数据库，如果 SQL 语句顺利执行后，使用 commit 执行提交数据库服务器，如果执行过程中有异常则需要使用 rollback 来对整个操作进行回滚操作。

在上下文管理协议中，通过两个方法 __enter__ 和 __exit__来实现上述功能，当然也可以使用 contextlib 中的 contextmanager 来实现上述功能。

在异步模式中，需要使用 __aenter__ 和 __aexit__ 来实现上述功能，调用时语法也变成了 async with 。

```

<h3>with 语法

 - 讲到上下文管理器，就不得不说到python的with语法。基本语法格式为：
```
with EXPR as VAR:
    BLOCK
    
```

<h3>异步模式的语法为：
```
async with EXPR as VAR:
    BLOCK
    
```
    
 - 其中 EXPR 必须返回一个对象，这个对象必须有 __enter__ 和 __exit__ 两个方法。异步模式下必须有 __aenter__ 和 __aexit__ 两个方法。__enter__ 和 __aenter__ 用于处理做预处理工作，返回的值会赋值到 VAR 上。__exit__ 和 __aexit__ 用于处理收尾工作。需要注意的是，__aenter__ 和 __aexit__ 必须为协程。

<h3>上下文代码的编写

 - 非异步模式：
```
class Connection(sqlite3.Connection):
    @contextmanager
    def trans(self):
        try:
            yield self
            self.commit()
        except Exception as e:
            self.rollback()
            raise e
            
```
<h3>上面这段代码执行通过 yield self 把数据库连接对象返回到 with 后面的代码，执行数据库的相关操作。如果顺利执行，调用 commit 来提交数据库；如果执行过程中存在问题，则调用 rollback 来回滚。

 - 异步模式：
```
class Connection(aiosqlite.Connection):

    async def findone(self, sql: str, params=None)->tuple:
        async with self.execute(sql, params)as cursor:
            return await cursor.fetchone()

    async def findall(self, sql: str, params=None):
        async with self.execute(sql, params)as cursor:
            return await cursor.fetchall()

    def trans(self):
        return Trans(self)

class Trans():
    __slots__ = ('_db',)

    def __init__(self, db: Connection):
        self._db = db

    async def __aenter__(self):
        return self._db

    async def __aexit__(self, exc_type, exc, tb):
        if exc_type:
            await self._db.rollback()
        else:
            await self._db.commit()
            
            
```
            
上述代码就是一个标准的上下文管理器的写法， __aenter__ 开始初始化，__aexit__ 执行清场工作。在清场工作中，如遇异常在进行回滚，否则进行提交。





