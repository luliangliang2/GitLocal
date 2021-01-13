# Nginx 代理配置

## 常用的nginx命令:<br>
 -  
  ``` Shell
  #关闭nginx
  ./nginx -s stop
  #重启nginx
   service nginx restart
  #配置文件修改重装载命令
  nginx -s reload
  #正常停止或关闭Nginx
  nginx -s quit  
  ```


## 安装nginx
- yum 安装源
 ```
yum -y install gcc pcre-devel zlib-devel openssl openssl-devel
 ```
- 下载“nginx-1.9.9.tar.gz”，移动到/usr/local/下 <br/>
 https://nginx.org/download/
 ```  Shell
 # 解压
 tar -zxvf nginx-1.9.9.tar.gz
 移动到/usr/local
 mv nginx-1.9.9 /usr/local/nginx
 # 配置
 ./configure  --prefix=/usr/local/nginx  --sbin-path=/usr/local/nginx/sbin/nginx --conf-path=/usr/local/nginx/conf/nginx.conf --error-log-path=/var/log/nginx/error.log  --http-log-path=/var/log/nginx/access.log  --pid-path=/var/run/nginx/nginx.pid --lock-path=/var/lock/nginx.lock  --user=nginx --group=nginx --with-http_ssl_module --with-http_stub_status_module --with-http_gzip_static_module --http-client-body-temp-path=/var/tmp/nginx/client/ --http-proxy-temp-path=/var/tmp/nginx/proxy/ --http-fastcgi-temp-path=/var/tmp/nginx/fcgi/ --http-uwsgi-temp-path=/var/tmp/nginx/uwsgi --http-scgi-temp-path=/var/tmp/nginx/scgi --with-pcre

 #编译安装
 make && make install
```


  - 出现以下错误
  usr/local/nginx/conf/nginx.conf'
  cp conf/nginx.conf '/usr/local/nginx/conf/nginx.conf.default'
  test -d '/var/run/nginx' 		|| mkdir -p '/var/run/nginx'
  test -d '/var/log/nginx' || 		mkdir -p '/var/log/nginx'
  test -d '/usr/local/nginx/html' 		|| cp -R html '/usr/local/nginx'
  test -d '/var/log/nginx' || 		mkdir -p '/var/log/nginx'
  make[1]: 离开目录“/software/nginx/nginx-1.9.9” <br/>
  #######  原因是缺少nginx用户 <br/>
  #######  解决办法
``` Shell
useradd -s /sbin/nologin -M
id nginx
```
查看nginx 启动状态 <br>
  ``` Shell
  netstat -tlunp | grep nginx
  ```

    #### 出现如下错误：

    #### checking for C compiler ... not found

    ####  ./configure: error: C compiler cc is not found

    根据错误提示，百度搜索 安装gcc
``` Shell
yum -y install gcc gcc-c++ autoconf automake make
```


---

## 配置nginx

  - 证书配置SSL证书开启 https:// 访问
  ``` Shell
  # server {
  #      listen     443  ssl;
  #      server_name  fw.cnedutech.com;
  #      ssl on;
  #      ssl_certificate      /cert/nginx.crt;
  #      ssl_certificate_key  /cert/nginx.key;
  ```

  - 精准匹配


  - 正常匹配
