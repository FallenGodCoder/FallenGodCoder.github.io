# mysql高级-服务器参数优化

## mysql参数

16G内存8核CPU

```ini
业务场景： 后台支持手机在线更新系统，db服务器内存16G，8核，dell的pc服务器。
qps： 200个左右
tps： 1个左右 一分钟50几个

#基础配置
datadir=/data/datafile
socket=/var/lib/mysql/mysql.sock
log-error=/data/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
character_set_server=utf8

#允许任意IP访问
bind-address = 0.0.0.0

#支持大小写
lower_case_table_names=1

#二进制配置
server-id = 1
log-bin = /data/log/mysql-bin.log
log-bin-index =/data/log/binlog.index
log_bin_trust_function_creators=1
expire_logs_days=7

#sql_mode定义了mysql应该支持的sql语法，数据校验等
#mysql5.0以上版本支持三种sql_mode模式：ANSI、TRADITIONAL和STRICT_TRANS_TABLES。
#ANSI模式：宽松模式，对插入数据进行校验，如果不符合定义类型或长度，对数据类型调整或截断保存，报warning警告。
#TRADITIONAL模式：严格模式，当向mysql数据库插入数据时，进行数据的严格校验，保证错误数据不能插入，报error错误。用于事物时，会进行事物的回滚。
#STRICT_TRANS_TABLES模式：严格模式，进行数据的严格校验，错误数据不能插入，报error错误。
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
#InnoDB存储数据字典、内部数据结构的缓冲池，16MB已经足够大了。
innodb_additional_mem_pool_size = 16M

#是否支持符号链接，即数据库或表可以存储在my.cnf中指定datadir之外的分区或目录，为0不开启
#symbolic-links=0
#InnoDB用于缓存数据、索引、锁、插入缓冲、数据字典等
#如果是专用的DB服务器，且以InnoDB引擎为主的场景，通常可设置物理内存的60%
#如果是非专用DB服务器，可以先尝试设置成内存的1/4
innodb_buffer_pool_size = 4G
#InnoDB的log buffer，通常设置为 64MB 就足够了
innodb_log_buffer_size = 64M
#InnoDB redo log大小，通常设置256MB 就足够了
innodb_log_file_size = 256M
#InnoDB redo log文件组，通常设置为 2 就足够了
innodb_log_files_in_group = 2
#共享表空间:某一个数据库的所有的表数据，索引文件全部放在一个文件中，默认这个共享表空间的文件路径在data目录下。默认的文件名为:ibdata1 初始化为10M。
#独占表空间:每一个表都将会生成以独立的文件方式来进行存储，每一个表都有一个.frm表描述文件，还有一个.ibd文件。其中这个文件包括了单独一个表的数据内容以及索引内容，默认情况下它的存储位置也是在表的位置之中。
#设置参数为1启用InnoDB的独立表空间模式，便于管理
innodb_file_per_table = 1
#InnoDB共享表空间初始化大小，默认是 10MB，改成 1GB，并且自动扩展
innodb_data_file_path = ibdata1:1G:autoextend
#设置临时表空间最大4G
innodb_temp_data_file_path=ibtmp1:500M:autoextend:max:4096M
#启用InnoDB的status file，便于管理员查看以及监控
innodb_status_file = 1
#当设置为0，该模式速度最快，但不太安全，mysqld进程的崩溃会导致上一秒钟所有事务数据的丢失。
#当设置为1，该模式是最安全的，但也是最慢的一种方式。在mysqld 服务崩溃或者服务器主机crash的情况下，binary log 只有可能丢失最多一个语句或者一个事务。
#当设置为2，该模式速度较快，也比0安全，只有在操作系统崩溃或者系统断电的情况下，上一秒钟所有事务数据才可能丢失。
innodb_flush_log_at_trx_commit = 1
#设置事务隔离级别为 READ-COMMITED，提高事务效率，通常都满足事务一致性要求
#transaction_isolation = READ-COMMITTED
#max_connections：针对所有的账号所有的客户端并行连接到MYSQL服务的最大并行连接数。简单说是指MYSQL服务能够同时接受的最大并行连接数。如果经常出现Too Many Connections的错误提示，则需要增大此值。
#max_user_connections : 针对某一个账号的所有客户端并行连接到MYSQL服务的最大并行连接数。简单说是指同一个账号能够同时连接到mysql服务的最大连接数。设置为0表示不限制。
#max_connect_errors：针对某一个IP主机连接中断与mysql服务连接的次数，如果超过这个值，这个IP主机将会阻止从这个IP主机发送出去的连接请求。遇到这种情况，需执行flush hosts。
#执行flush host或者 mysqladmin flush-hosts，其目的是为了清空host cache里的信息。可适当加大，防止频繁连接错误后，前端host被mysql拒绝掉
#在 show global 里有个系统状态Max_used_connections,它是指从这次mysql服务启动到现在，同一时刻并行连接数的最大值。它不是指当前的连接情况，而是一个比较值。如果在过去某一个时刻，MYSQL服务同时有10
00个请求连接过来，而之后再也没有出现这么大的并发请求时，则Max_used_connections=1000.请注意与show variables 里的max_user_connections的区别。#Max_used_connections / max_connections * 100% ≈ 85%
max_connections=600
max_connect_errors=1000
max_user_connections=400
#设置临时表最大值，这是每次连接都会分配，不宜设置过大 max_heap_table_size 和 tmp_table_size 要设置一样大
max_heap_table_size = 100M
tmp_table_size = 100M
#每个连接都会分配的一些排序、连接等缓冲，一般设置为 2MB 就足够了
sort_buffer_size = 8M
join_buffer_size = 8M
# join_buffer_size 这个参数为什么没有看到,必须设置值，join_buffer_size = 8M 必须补上，这个是跟join table 关联的，很重要。
read_buffer_size = 8M
read_rnd_buffer_size = 8M
#建议关闭query cache，有些时候对性能反而是一种损害
query_cache_size = 0
#如果是以InnoDB引擎为主的DB，专用于MyISAM引擎的 key_buffer_size 可以设置较小，8MB 已足够
#如果是以MyISAM引擎为主，可设置较大，但不能超过4G
key_buffer_size = 8M
#设置连接超时阀值，如果前端程序采用短连接，建议缩短这2个值，如果前端程序采用长连接，可直接注释掉这两个选项，是用默认配置(8小时)
#interactive_timeout = 120
#wait_timeout = 120
#InnoDB使用后台线程处理数据页上读写I/0请求的数量,允许值的范围是1-64
#假设CPU是2颗4核的，且数据库读操作比写操作多，可设置
#innodb_read_io_threads=5
#innodb_write_io_threads=3
#通过show engine innodb status的FILE I/O选项可查看到线程分配
#设置慢查询阀值，单位为秒
long_query_time = 120
slow_query_log=1 #开启mysql慢sql的日志
log_output=table,File #日志输出会写表，也会写日志文件，为了便于程序去统计，所以最好写表
slow_query_log_file=/data/log/slow.log
##针对log_queries_not_using_indexes开启后，记录慢sql的频次、每分钟记录的条数
#log_throttle_queries_not_using_indexes = 5
##作为从库时生效,从库复制中如何有慢sql也将被记录
#log_slow_slave_statements = 1
##检查未使用到索引的sql
#log_queries_not_using_indexes = 1
#快速预热缓冲池
innodb_buffer_pool_dump_at_shutdown=1
innodb_buffer_pool_load_at_startup=1
#打印deadlock日志
innodb_print_all_deadlocks=1

innodb_read_io_threads=16
innodb_read_write_threads=16 
```

## centos服务器参数

[扩展阅读](http://www.voidcn.com/article/p-kqtkbhjk-bws.html)

### sysctl.conf

/etc/sysctl.conf内核参数

```ini
net.core.somaxconn=65535,这个参数决定每个端口最大监听队的长度（这个参数默认值比较小，当负载很大的服务器来说是不够的，一般改成2048或更大）
net.core.netdev_max_backlog=65535 每个网络接口接收数据包的速率比内核处理数据包速率快的时候，允许为发送到队列中的数据包最大数目
net.ipv4.tcp_max_syn_backlog=65535 还未获得对方连接的请求，可保存在队列中的最大数目，超过会被抛弃

net.ipv4.tcp_fin_timeout = 10 用于控制tcp处理的等待状态时间，对于连接比较频繁系统，通常有大量连接处于等待状态。用于减少这个状态的时间，加快tcp回收速度
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1
# 这三个参数主要用于加快tcp连接的回收

net.core.wmem_default=87380
net.core.wmem_max=16777216
net.core.rmem_default=87380
net.core.rmem_max=16777216
# 以上四个参数用于tcp连接的接收和发送缓存区大小的默认值和最大值

net.ipv4.tcp_keepalive_time=120
net.ipv4.tcp_keepalive_intvl=30
net.ipv4.tcp_keepalive_probes=3
# 以上三个参数用于减少失效连接所占用的tcp系统资源，加快资源回收效率

kernel.shmmax=4294967295
# Linux内核参数中最重要的参数之一，用于定义单个内存段的最大值。
# kernel.shmmax = 4G
# 1. 这个参数应该设置的足够大，以便能在一个共享内存段下容纳下整个的Innodb缓冲池的大小。

vm.swappiness=1
# 这个参数当内存不足的时候，如果开启就会使用本地硬盘划分出来的交换内存，这个时候回对性能产生比较明显的影响。MysQL一般都设置为1，尽可能不用swap分区。在MySQL服务器上保留交换分区还是很必要的，但是要控制何时十一哦那个交换分区。如果关闭交换分区，可能会降低操作系统的性能，以及容易造成内存溢出、崩溃，或者被OS直接Kill掉。
# free -m 可以看到swap交换分区的情况。
#vm.swappiness = 0 # 高速Linux内核除非非虚拟内存完全满了，否则不要使用交换区。
```



### limit.conf

/etc/security/limit.conf

这个文件实际上是Linux PAM，也就是插入式认证模块的配置文件。增加文件数的限制。

```
* soft nofile 65535
* hard nofile 65535
# 上面两行加到limit.conf文件的末尾就可以了。

* 表示对所有用户生效
soft 指的是当前系统生效的设置。
hard 表明系统中所能设定的最大值。
nofile 表示所限制的资源是打开文件的最大数目。
65535 就是限制的数量。
# 结论：把可打开的文件数量增加到65535个以保证可以打开足够多的文件句柄。
# 注意：这个文件的修改需要重启系统才可以生效。
```

## SysBench性能基准测试工具

SysBench是一个模块化的、跨平台、多线程基准测试工具，主要用于评估测试各种**不同系统参数**下的**数据库负载情况**。它主要包括以下几种方式的测试：
       1、cpu性能
       2、磁盘io性能
       3、调度程序性能
       4、内存分配及传输速度
       5、POSIX线程性能
       6、数据库性能(OLTP基准测试)      

目前sysbench主要支持 MySQL,pgsql,oracle这3种数据库。

[sysbench测试mysql性能（TPS、QPS、IOPS）](https://blog.csdn.net/qq_36357820/article/details/80079012)