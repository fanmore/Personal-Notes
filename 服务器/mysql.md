## 备份数据库

```
mysqldump -h127.0.0.1 -uroot -pfanjun --databases fan > a.sql
```

## 还原数据库

```
mysql -h127.0.0.1 -uroot -ppass myweb < 备份文件
```
