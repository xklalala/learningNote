    MyISAM存储引擎由MYD和MYI组成

### myisam 特性
> 1. 并发性与锁级别

    使用表级锁
    读写混合并发性支持不太好

> 2. 表损坏修复

    check table tableName
    repair table tableName

> 3.支持索引类型

    1. 全文索引
    2. 

> 4. 支持数据压缩

        myisampack

        压缩的表不能写

> 5. 限制

    1. 版本 < mysql5.0时默认表大小为4G
        如果存储大表则要修改MAX_Rows和AVG_ROW_LENGTH

    2. 版本 > mysql5.0时默认支持为256TB


> 6. 适用场景

    1. 非事务型应用
    2. 只读类应用
    3. 空间类应用
    

