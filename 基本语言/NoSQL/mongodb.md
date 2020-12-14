数据由键值对组成

mogodb数据类型：double、string、Object、Object Id、Array、Boolean、Data、Null等

sort()：1正序、-1倒序

limit(100)：分页，只展示100条

查询语句：

```sql
db.student.find({'name':'zhangsan','gender':'F','age':{$gt:18}})
db.student.find({
                $or:[
                  {'class':'A01'},{'class':'B02'}
                ]
                })
db.student.find({'name':'hanna',$or:[{'class':'A01'},{'class':'B02'}])
db.student.find({'class':'A01'}).srot({'student_id':1})
```

