# docker的命令

 - 出现问题
  - docker容器无法yum(可能是在切换网络的情况下)
  ```
  systemctl restart docker
  ```
 - 查看docker镜像信息
 ```Shell
 # docker info
 ```

 - 如何编写dockerfile

 ```Shell
#docker images -q 查看所有的已经存在的镜像
#sudo docker rm $(sudo docker ps -a -q) 清空所有容器
 ```

# docker
  - image >> containers <br>
    镜像是静态的不会运行，容器是动态的有生命周期

  - volumes:卷 <br>
  - plugins:插件


  docker的加速方式 <br>
  docker-cn <br>
  阿里云加速器 <br>
  中国科技大学
