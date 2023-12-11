# Data_SQLServer



### 五.Sqlserver线上问题排查

#### 1.[利用sys.sysprocesses检查SqlServer的阻塞和死锁](https://www.cnblogs.com/w-zoe/p/9011064.html)

```sql
select *
from sys.sysprocesses
where blocked <> 0;

sp_WhoIsActive;

EXEC sp_who2 'active';
```

