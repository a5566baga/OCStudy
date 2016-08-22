# FMDB的使用学习

---

## Cocoapods的安装

> #### 1、更换ruby的资源，换成 gem sources -a https:\/\/gems.ruby-china.org
> 
> #### 2、执行命令sudo gem install -n \/usr\/local\/bin cocoapods --pre
> 
> #### 3、执行一下pod setup
> 
> #### 4、把master的内容放在\/Users\/你的用户名\/.cocoapods\/repos路径下



---

## FMDB的使用

#### 步骤：

> #### 1、创建路径
> 
> #### 2、创建一个队列
> 
> #### 3、创建数据\/打开库
> 
> #### 4、创建SQL语句
> 
> #### 5、执行语句
> 
> #### 6、对结果进行操作

#### 举例说明

##### 创建数据库

```
    NSString * path = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES).lastObject stringByAppendingPathComponent:@"student.sqlite"];
    
    NSLog(@"%@", path);
    _queue = [FMDatabaseQueue databaseQueueWithPath:path];
    
    [_queue inDatabase:^(FMDatabase *db) {
       BOOL result  =  [db executeUpdate:@"CREATE TABLE IF NOT EXISTS t_student(id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, age INTEGER, score REAL);"];
        if (result) {
            NSLog(@"CREATE SUCCESS");
        }else{
            NSLog(@"CREATE FAILED");
        }
    }];
```

##### 插入数据

```
    [_queue inDatabase:^(FMDatabase *db) {
        [db executeUpdate:@"INSERT into t_student(name, age ,score) VALUES (?, ? ,?);", stup.name, @(stup.age), @(stup.score)];
    }];
```

##### 删除数据

```
    [_queue inDatabase:^(FMDatabase *db) {
        [db executeUpdate:@"DELETE FROM t_student WHERE age = ?;", age];
    }];
```

