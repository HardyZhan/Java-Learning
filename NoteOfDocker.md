# Docker

## docker的常用命令

### 帮助命令

* `docker version` 显示docker的版本信息
* `docker info` 显示docker的系统信息，包括镜像和容器数量
* `docker 命令 --help` 帮助命令

帮助文档的地址 ：https://docs.docker.com/reference/

### 镜像命令

* `docker images` 查看所有本地的主机上的镜像

  * 可选项
    * -a, --all 列出所有镜像
    * -q,--quiet 只显示镜像的id

  ```shell
  [root@HardyZ /]# docker images
  REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
  hello-world   latest    feb5d9fea6a5   6 months ago   13.3kB
  ```

  * REPOSITORY 镜像的仓库源
  * TAG 镜像的标签
  * IMAGE ID 镜像的id
  * CREATED 镜像的创建时间
  * SIZE 镜像的大小

- `docker search 镜像名` 从仓库中查找对应镜像

  ```shell
  [root@HardyZ /]# docker search mysql
  NAME                             DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
  mysql                            MySQL is a widely used, open-source relation…   12422     [OK]       
  mariadb                          MariaDB Server is a high performing open sou…   4786      [OK]       
  mysql/mysql-server               Optimized MySQL Server Docker images. Create…   918                  [OK]
  percona                          Percona Server is a fork of the MySQL relati…   575       [OK]       
  phpmyadmin                       phpMyAdmin - A web interface for MySQL and M…   507       [OK]       
  mysql/mysql-cluster              Experimental MySQL Cluster Docker images. Cr…   93                   
  ```

  - `--filter=STARS=3000` 加上该参数可以过滤出镜像的STARS大于3000的

- `docker pull 目标镜像[:tag]` 下载镜像，如果不写tag，默认就是latest

- `docker rmi -f 镜像ID或者名称` 删除指定镜像

  - `docker rmi -f $(docker images -aq)` 递归删除全部容器

### 容器命令

#### 新建容器并启动

* `docker pull centos` 下载一个centos镜像

* `docker run [可选参数] image` 新建容器并启动

  * --name="Name"  容器名字，tomcat01、tomcat02，用来区分容器

  * -d 后台方式运行

  * -it 使用交互方式进行，进入容器查看内容

  * -p 指定容器的端口，-p 8080:8080

    * -p ip:主机端口：容器端口
    * -p 主机端口：容器端口（常用）
    * -p 容器端口
    * 容器端口

  * -P 随机指定端口

    **演示**

  ```shell
  [root@HardyZ /]# docker run -it centos /bin/bash
  [root@b7d2e703028a /]# ls
  bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
  [root@b7d2e703028a /]# exit
  exit
  ```

#### 列出所有运行的容器

* `docker ps` 列出当前正在运行的容器
  * -a 列出当前正在运行的容器+带出历史运行过的容器
  * -n=? 显示最近创建的容器，?处用数字替代可以控制输出的容器数量
  * -q 只显示容器的编号

#### 退出容器

* `exit` 推出容器并停止
  * Ctrl + p + q 推出容器但不停止容器

#### 删除容器

* `docker rm 容器id` 删除指定容器，不能删除正在运行的容器，如果要强制删除，需要加-f
  * `docker rm -f $(docker ps -aq)` 删除所有的容器
  * `docker ps -a -q|xargs docker rm` 删除所有的容器

#### 启动和停止容器

* `docker start 容器id` 启动容器
* `docker restart 容器id` 重启容器
* `docker stop 容器id` 停止当前正在运行的容器
* `docker kill 容器id` 强制停止当前的容器

### 常用其他命令

#### 后台启动容器

* `docker run -d centos` 
  * 问题：发现centos停止了
  * 原因：docker容器使用后台运行就必须要有一个前台进程，docker发现没有应用，就会自动停止
  * nginx容器启动后，发现自己没有提供服务，就会立刻停止

#### 查看日志

* `docker logs -tf --tail number 容器`  
  * -tf  显示日志
  * --tail number 要显示的日志条数

#### 查看容器中进程信息

* `docker top 容器id` 显示指定容器中的进程信息

#### 查看镜像的元数据

* `docker inspect 容器id`

#### 进入当前正在运行的容器

我们通常容器都是使用后台方式运行的，需要进入容器修改一些配置

* `docker exec -it 容器id bashshell`  进入容器后开启一个新的终端，可以在里面操作（常用）
* `docker attach 容器id` 进入容器正在执行的终端，不会启动新的进程

#### 从容器内拷贝文件到主机上

* `docker cp 容器id:容器内路径 目的的主机路径`
* 测试

```shell
[root@HardyZ home]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
0d0eea7c3c06   centos    "/bin/bash"   5 minutes ago   Up 5 minutes             condescending_noether
[root@HardyZ home]# docker attach 0d0eea7c3c06
[root@0d0eea7c3c06 /]# cd /home
[root@0d0eea7c3c06 home]# ls
[root@0d0eea7c3c06 home]# touch test.java
[root@0d0eea7c3c06 home]# exit
exit
[root@HardyZ home]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@HardyZ home]# docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED             STATUS                         PORTS     NAMES
0d0eea7c3c06   centos        "/bin/bash"   6 minutes ago       Exited (0) 9 seconds ago                 condescending_noether
b7d2e703028a   centos        "/bin/bash"   43 minutes ago      Exited (0) 42 minutes ago                intelligent_jepsen
f0254862ec53   hello-world   "/hello"      About an hour ago   Exited (0) About an hour ago             unruffled_jemison
c879cac00cf2   hello-world   "/hello"      47 hours ago        Exited (0) 47 hours ago                  lucid_jackson
[root@HardyZ home]# docker cp 0d0eea7c3c06:/home/test.java /home
[root@HardyZ home]# ls
test.java  www  zhanhaodi  zhanhaodi.java
```

