### 镜像命令：
1. 获取镜像：
```
 docker pull eg. sudo docker pull ubuntu
```
2. 查看镜像信息
```
docker images 列出列出本地主机上已有的镜像
```
3. 搜寻镜像 
  docder search 命令可以搜索远端仓库中共享的镜像，默认搜寻 Docker Hub 官方仓库中的镜像
```
$ sudo docker search mysql
```
4. 删除镜像
```
$ sudo docker rmi imageid/label
```
5. 创建镜像
创建镜像的方法有三种，基于已有镜像的容器创建、基于本地模板导入、基于Dockerfile创建
  1、基于已有镜像容器创建，主要是使用docker commit命令
  $ sudo docker 

### 容器的常用命令：

创建容器：
  使用 docker create 命令新建一个容器
  $sudo docker create -it ubuntu:latest (新建的容器处于停止状态，可以用docker start来启动它)
  
1. 使用容器运行mysql镜像：
  $ sudo docker run --name mysqldb -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest
2、进入容器：
  attach命令：
  exec命令：
  $ sudo docker exec -ti mysqldb bash   
    -t  分配一个伪终端
    -i  即使没有附加也保持STDIN打开
3、终止容器
  $sudo docker stop mysqldb
  处于终止的容器可以通过 docker start 命令来重新启动
  $ sudo docker start mysqldb
4、在容器内使用ps命令查看进程
  如果出现 bash: man: command not found，镜像没有打包ps命令，使用如下命令安装
   apt-get update && apt-get install procps
5、删除容器
  可以使用docker rm 命令删除处于终止状态的容器，
  ```
  $ sudo docker ps -a
  [sudo] password for ***: 
  CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
  a55c7f0105f1        mysql:latest        "docker-entrypoint.s…"   About an hour ago   Up 39 minutes       0.0.0.0:3306->3306/tcp, 33060/tcp   mysqldb  
  liubing@iZj6cc6u1iybgbu0vkx6vlZ:~$ sudo docker stop mysqldb
  mysqldb
  liubing@iZj6cc6u1iybgbu0vkx6vlZ:~$ sudo docker rm a55c7f0105f1
  a55c7f0105f1
```
-  如果要删除一个运行中的容器，可以添加 -f 参数，docker会发送SIGKILL 信号给容器，终止其中的应用
```
$ sudo docker rm -f mysqldb
```

### 容器互联：

-  先创建一个新的 Docker 网络。
```
$ docker network create -d bridge my-net
```
- 连接容器 

  运行一个容器并连接到新建的 my-net 网络
```
$ docker run -it --rm --name busybox1 --network my-net busybox sh
```
- 打开新的终端，再运行一个容器并加入到 my-net 网络
```
  $ docker run -it --rm --name busybox2 --network my-net busybox sh
```

### Docker mysql 把数据存储在本地目录
  1. 加上-v参数
```
$ docker run -d -e MYSQL_ROOT_PASSWORD=admin --name mysql -v /data/mysql/data:/var/lib/mysql -p 3306:3306 mysql 1
```
2. 还可以指定配置文件
```
docker run -d -e MYSQL_ROOT_PASSWORD=admin --name mysql -v /data/mysql/my.cnf:/etc/mysql/my.cnf -v /data/mysql/data:/var/lib/mysql -p 3306:3306 mysql 1
```
- mysql 客户端不能连接解决办法：
1. 
```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
     mysql> FLUSH PRIVILEGES;
```
2. 改表法。
```  mysql -u root -pvmwaremysql>use mysql;　　
  mysql>update user set host = '%' where user = 'root';　　
  mysql>select host, user from user;　　
  mysql>FLUSH RIVILEGES
```
3. 授权法
```
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%'IDENTIFIED BY 'mypassword' WITH GRANT OPTION;　　
FLUSH RIVILEGES
```

- 一条命令实现停用并删除容器：
```
docker stop $(docker ps -q) & docker rm $(docker ps -aq)
```
