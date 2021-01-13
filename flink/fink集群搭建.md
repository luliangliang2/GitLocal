# 搭建fink

## 下载flink   
-  https://mirrors.cloud.tencent.com/apache/flink/  

## 解压flink
```shell
tar -zxvf flink.tgz
```

## 配置环境
```shell
export FLINK_HOME=/opt/flink
export PATH=$FLINK_HOME/bin:$PATH
#使环境生效
source /etc/profile
```

## 配置flink
###### 修改flink/conf/masters，slaves，flink-conf.yaml
```shell
[admin@master conf]$ sudo vi masters
master:8081
[admin@master conf]$ sudo vi workers
node1
node2
[admin@master conf]$ sudo vi flink-conf.yaml
taskmanager.numberOfTaskSlots：2
jobmanager.rpc.address: master
```

## 将配置拷贝到其他的节点机器上
```shell
scp masters  node1:/opt/flink/conf/
scp workers node1:/opt/flink/conf/   
scp flink-conf.yaml node1:/opt/flink/conf/
scp masters  node2:/opt/flink/conf/
 scp workers node2:/opt/flink/conf/
scp flink-conf.yaml node2:/opt/flink/conf/     
```
## 启动集群
```shell
 ./bin/start-cluster.sh
```


# 高可用
  -  conf/flink-conf.yaml  
    ```shell
    high-availability: zookeeper

    high-availability.storageDir: hdfs://tl/flink/ha/

    # 指定高可用模式（必须）
    high-availability.zookeeper.quorum: master:2181,node1:2181,node2:2181

    # jobManager元数据保存在文件系统storageDir中，只有指向此状态的指针存储在zookeeper中（必须）
    high-availability.zookeeper.path.root: /flink

    # 根据zookeeper节点，在该节点下放置所有集群节点（推荐）
    high-availability.cluster-id: /flinkCluster

    #================================================================
    # Fault tolerance and checkpointing
    #================================================================
    state.backend: filesystem

    state.checkpoints.dir: hdfs://tl/flink/checkpoints

    state.savepoints.dir: hdfs://tl/flink/checkpoints

    ```
  - 修改配置文件 conf/workers  
    master  
    node1  
    node2
  - 修改配置文件 conf/master  
    master:8081  
    node1:8081
  - 修改配置文件 conf/zoo.cfg  
    #ZooKeeper quorum peers  
  server.1=master:2888:3888  
  server.2=node1:2888:3888  
  server.3=node2:2888:3888


# 与Hadoop集成
  - 直接修改配置文件bin/config.sh  
  ```shell
  # YARN Configuration Directory, if necessary
  DEFAULT_YARN_CONF_DIR="/opt/hadoop/share/hadoop/yarn"
  # Hadoop Configuration Directory, if necessary
  DEFAULT_HADOOP_CONF_DIR="/opt/hadoop/etc/hadoop"
  ```  

- 添加hadoop依赖
flink-shaded-hadoop-2-uber-2.8.3-10.0.jar  

  https://repo.maven.apache.org/maven2/org/apache/flink/flink-shaded-hadoop-2-uber/2.8.3-10.0/  
  下载jar包放到/flink/lib目录下
