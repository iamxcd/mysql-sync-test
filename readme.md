# mysql 主从复制试验
采用docker模拟多个服务器.

参考:https://www.cnblogs.com/songwenjie/p/9371422.html


## 导入mysql配置文件
```
// 最开始是想将config/db 下面的配置分别映射到容器内,但是权限不对,mysql启动时配置被忽略.chmod也修改不到

docker cp config/db/master/conf.d/mysqld.cnf mysqltest_master_1:/etc/mysql/conf.d/mysqld.cnf
docker cp config/db/slave/conf.d/mysqld.cnf mysqltest_slave_1:/etc/mysql/conf.d/mysqld.cnf

docker-compose exec master bash
然后进容器 chmod 644 mysqld.cnf 这种方式就能改了.

```

![运行情况](./imgs/微信截图_20200624152317.png)

## 查看日志是否开启
```
show master status
```
![sql](./imgs/微信截图_20200624151932.png)



## 在从数据操作
```
change master to master_host='172.20.0.4', master_user='root', master_password='root', master_port=3306, master_log_file='mysql-bin.000004', master_log_pos= 747, master_connect_retry=30;

start slave //开启主从同步
show slave status // 查看主从同步状态
```

## 最后测试
主库创建张表,插入数据 从库也有相应数据