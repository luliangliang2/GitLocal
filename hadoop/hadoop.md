#  hadoop集群搭建

- 下载hadoop-2.7.5.tar.gz

- 解压到要部署的机器  
放在 /opt 目录下面

- mv hadoop-2.7.5 hadoop

- 配置hadoop环境变量  

  ```Shell
  export HADOOP_HOME=/opt/hadoop/
  export PATH=$PATH:$HADOOP_HOME/bin
  export PATH=$PATH:$HADOOP_HOME/sbin
  export HADOOP_MAPRED_HOME=$HADOOP_HOME
  export HADOOP_COMMON_HOME=$HADOOP_HOME
  export HADOOP_HDFS_HOME=$HADOOP_HOME
  export YARN_HOME=$HADOOP_HOME
  export HADOOP_ROOT_LOGGER=INFO,console
  export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
  export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
  ```
- 修改/opt/hadoop/etc/hadoop/hadoop-env.sh  

  改为实际的JDK地址  
  export  JAVA_HOME=/usr/local/jdk

- 修改/opt/hadoop/etc/hadoop/slaves  
  此处为2个从节点，node1,node2

- 修改/opt/hadoop/etc/hadoop/core-site.xml  
    这里应该是设置主节点
  ``` Shell
  <configuration>
    <property>
      <name>fs.defaultFS</name>
      <value>hdfs://master:9000</value>
      </property>
      <property>
      <name>io.file.buffer.size</name>
      <value>131072</value>
    </property>
    <property>
      <name>hadoop.tmp.dir</name>
      <value>/opt/hadoop/tmp</value>
    </property>
  </configuration>
  ```
- 修改/opt/hadoop/etc/hadoop/hdfs-site.xml  
配置secondary namenode

```Shell
  <configuration>
    <property>
      <name>dfs.namenode.secondary.http-address</name>
      <value>node1:50090</value>
    </property>
    <property>
      <name>dfs.replication</name>
      <value>3</value>
    </property>
    <property>
      <name>dfs.namenode.name.dir</name>
      <value>file:/opt/hadoop/hdfs/name</value>
    </property>
    <property>
      <name>dfs.datanode.data.dir</name>
      <value>file:/opt/hadoop/hdfs/data</value>
    </property>
  </configuration>
```
- 修改/opt/hadoop/etc/hadoop/mapred-site.xml  
  如果没有mapred-site.xml  
  可以执行命令，复制一个mapred-site.xml  
  cp  mapred-site.xml.template   mapred-site.xml

  ```Shell
  <configuration>
   <property>
      <name>mapreduce.framework.name</name>
      <value>yarn</value>
    </property>
    <property>
            <name>mapreduce.jobtracker.http.address</name>
            <value>master:50030</value>
    </property>
    <property>
            <name>mapreduce.jobhistory.address</name>
            <value>master:10020</value>
    </property>
    <property>
            <name>mapreduce.jobhistory.address</name>
            <value>master:19888</value>
    </property>

    <property>
            <name>mapred.job.tracker</name>
            <value>master:9001</value>
    </property>
    </configuration>
  ```

- 修改/opt/hadoop/etc/hadoop/yarn-site.xml  

  ``` Shell
  <configuration>
     <property>
         <name>yarn.nodemanager.aux-services</name>
         <value>mapreduce_shuffle</value>
     </property>
     <property>
         <name>yarn.resourcemanager.address</name>
         <value>master:8032</value>
     </property>
     <property>
         <name>yarn.resourcemanager.scheduler.address</name>
         <value>master:8030</value>
     </property>
     <property>
         <name>yarn.resourcemanager.resource-tracker.address</name>
         <value>master:8031</value>
     </property>
     <property>
         <name>yarn.resourcemanager.admin.address</name>
         <value>master:8033</value>
     </property>
     <property>
         <name>yarn.resourcemanager.webapp.address</name>
         <value>master:8088</value>
     </property>
</configuration>

  ```

  至此，node1上的hadoop配置完成，将/opt/hadoop-2.7.5全部拷贝到node2,node3
  scp  -r /opt/hadoop-2.7.5  node2:/opt/
  scp  -r /opt/hadoop-2.7.5  node3:/opt


- 集群格式化  
分别在node1,node2,node3 执行  
hadoop namenode -format  
如果没有报错，则格式完成。

- 启动集群  
在node1上，启动hadoop集群  
cd /opt/hadoop/sbin  
./start-all.sh
