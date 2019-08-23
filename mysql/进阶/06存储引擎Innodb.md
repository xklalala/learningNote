`innodb是mysql5.5及以后版本默认存储引擎`

    innodb支持事务
    innodb适用表空间进行数据存储

    配置：innodb_file_per_table
        on:独立表空间：tableName.ibd
        off:系统表空间：ibdataX

> 系统表空间和独立表空间的选择

    比较：
        1. 系统表空间无法简单的收缩文件大小
        2. 独立表空间可以通过optimize table命令收缩系统文件
        3. 系统表空间会产生IO瓶颈
        4. 独立表空间可以同时向多个文件刷新数据
        5. 建议使用独立表空间
        mysql5.6以后独立表空间是默认配置
    