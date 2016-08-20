# SQLite学习

---

## SQLite数据库的使用

### 安装

> #### 对dmg文件直接安装即可

### 视图创建表

![](/assets/数据库创建.png)

#### 添加字段

![](/assets/创建字段.png)

#### 添加主键

#### 添加数据

![](/assets/添加数据.png)

#### 添加外键

![](/assets/外键约束.png)

#### 添加唯一约束

![](/assets/唯一约束.png)

#### 添加检查约束

![](/assets/检查约束.png)

### SQL语句使用

#### 创建数据表

```
CREATE TABLE IF NOT EXISTS t_student(
    id integer PRIMARY KEY AUTOINCREMENT,
    name text,
    age integer,
    score real
)
```

#### 插入数据

```
INSERT INTO t_student(name, age, score) VALUES('Rose', 22, 66);
```

#### 删除数据

```
DELETE FROM t_student
```

#### 删除表

```
DROP TABLE IF EXISTS t_student;
```

#### 修改表结构

```
ALTER TABLE table_name
ADD column_name datatype
```

```
ALTER TABLE table_name 
DROP COLUMN column_name
```

```
ALTER TABLE table_name
ALTER COLUMN column_name datatype
```

#### 更新数据

```
UPDATE t_student SET age = 18, score = 100;
```

#### 查找数据

```
SELECT * FROM t_class 
WHERE t_class = 1
ORDER BY class_id
```

---

## 代码实现

### 步骤：

> #### 1、开启一个数据库
> 
> #### 2、创建CQUD语句
> 
> > 1、创建SQL语句
> > 
> > 2、检查语句返回结果
> > 
> > 3、查询的话要先检查SQL语句的正确性



