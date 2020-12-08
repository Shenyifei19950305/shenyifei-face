## MySQL基础语句

### 结构化查询语句分类

![image-20200620161046519](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200620161046519.png)

### 数据字段属性

**UnSigned**

- 无符号的
- 声明该数据列不允许负数 .

**ZEROFILL**

- 0填充的
- 不足位数的用0来填充 , 如int(3),5则为005

**Auto_InCrement**

- 自动增长的 , 每添加一条数据 , 自动在上一个记录数上加 1(默认)

- 通常用于设置**主键** , 且为整数类型

- 可定义起始值和步长

- - 当前表设置步长(AUTO_INCREMENT=100) : 只影响当前表
  - SET @@auto_increment_increment=5 ; 影响所有使用自增的表(全局)

**NULL 和 NOT NULL**

- 默认为NULL , 即没有插入该列的数值
- 如果设置为NOT NULL , 则该列必须有值

**DEFAULT**

- 默认的
- 用于设置默认值
- 例如,性别字段,默认为"男" , 否则为 "女" ; 若无指定该列的值 , 则默认值为"男"的值

算了，直接找达内的那个笔记直接看得了

一般语句是没问题的，说几个难点的

Join连接查询

INNER JOIN 内连接 查询两个表中的交集

LEFT JOIN 左连接 以左表为基础 右边表来一一匹配，右边匹配不上的以NULL填充

RIGHT JOIN 右连接 相反 一样的用法

举个例子：

　A表　　　　　　　　　　

　　id　  name　　

　　1　　小王

　　2　　小李

　　3　　小刘

　　B表

　　id　　A_id　　job

　　1　　2　　　　老师

　　2　　4　　　　程序员
内连接：（只有2张表匹配的行才能显示）

select a.name,b.job from A a  inner join B b on a.id=b.A_id

　　只能得到一条记录

　　小李　　老师
左连接：（左边的表不加限制）

select a.name,b.job from A a  left join B b on a.id=b.A_id

　　三条记录

　　小王　　null

　　小李　　老师

　　小刘　　null
右连接：（右边的表不加限制）

select a.name,b.job from A a  right join B b on a.id=b.A_id

　　两条记录

　　小李　　老师

　　null　　程序员

难一点的 用了Join和子查询 这个 假如

```
-- 查询课程为 高等数学-2 且分数不小于80分的学生的学号和姓名
```

有两种表达

```sql
SELECT s.studentno,studentname
FROM student s
INNER JOIN result r
ON s.`StudentNo` = r.`StudentNo`
INNER JOIN `subject` sub
ON sub.`SubjectNo` = r.`SubjectNo`
WHERE subjectname = '高等数学-2' AND StudentResult>=80
```

```sql
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80 AND subjectno=(
   SELECT subjectno FROM `subject`
   WHERE subjectname = '高等数学-2'
)
```

```sql
SELECT studentno,studentname FROM student WHERE studentno IN(
   SELECT studentno FROM result WHERE StudentResult>=80 AND subjectno=(
       SELECT subjectno FROM `subject` WHERE subjectname = '高等数学-2'
  )
)
```