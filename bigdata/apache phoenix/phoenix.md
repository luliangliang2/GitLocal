apache phoenix查询缓慢问题

现象：phoenix刚建表时查找很快，随着数据导入越来越多，查询越来越缓慢，执行explain这个表的计划都需要好几秒，但在hbase shell里查询很快

问题定位：这个是由于system.static表数据量太大造成，每次查询都会去读这张表数据

解决方案：修改org.apache.phoenix.coprocessor.MetaDataEndpointImpl，注解这句话：

//stats = StatisticsUtil.readStatistics(statsHTable, physicalTableName.getBytes(), clientTimeStamp);

重新编译phoenix源码，替换所有机器hbase/lib下phoenix-server、phoenix-core包，然后重启hbase集群即可。

明显phoenix在设计时没有考虑到system.static表数据量太大情况。
