## 查看数据库
```
show databases;
```

## 创建数据库
```
create databases name character set name;
```
character set ： 设置字符集，如 utf8 （可以通过 show character set 查看数据库支持的字符集）

## 删除数据库
```
drop database name;
```

## 外键
创建
```
constraint fk_name foreign key(name) references tablename(name)
```

修改时创建
```
alter table name
add constraint fk_name foreign key(name) references tablenaem(name)
```

删除
```
alter table name
drop foreign key fk_name
```
