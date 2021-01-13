# zookeeper集群搭建

 -  # 准备工作安装jdk

 -  # 下载zookeeper
   http://archive.apache.org/dist/zookeeper/zookeeper-3.5.5/

-  # 解压到  /opt/zookeeper
  tar -vxf zookeeper.tar

- # 进入到 /opt/zookeeper/

  cp conf/zoo_sample.cfg conf/zoo.cfg

- # 配置zoo.cfg
  dataLogDir=/opt/zookeeper/logs

  dataDir=/opt/zookeeper/data

  server.1= 192.168.1.148:2888:3888

  server.2= 192.168.1.149:2888:3888

  server.3= 192.168.1.150:2888:3888

- # 创建ServerID标识
 服务器上面创建myid文件，并设置值为1，同时与zoo.cfg文件里面的server.3保持一致，如下
 echo "1" > /opt/zookeeper/data/myid

 服务器上面创建myid文件，并设置值为1，同时与zoo.cfg文件里面的server.3保持一致，如下
 echo "2" > /opt/zookeeper/data/myid

 服务器上面创建myid文件，并设置值为1，同时与zoo.cfg文件里面的server.3保持一致，如下
 echo "3" > /opt/zookeeper/data/myid

- # 把zookeeper加入到环境变量

  echo -e "# append zk_env\nexport PATH=$PATH:/opt/zookeeper/bin" >> /etc/profile

- # 启动每一台节点
/opt/zookeeper/bin/zkServer.sh start

 Zookeeper节点启动不了可能原因：zoo.cfg配置文件有误、iptables没关。

- # 启动完成之后查看每个节点的状态

 /opt/zookeeper/bin/zkServer.sh status

- # Zookeeper集群连接
 /opt/zookeeper/bin/zkCli.sh -server master:2181

- # 退出Zookeeper
quit
