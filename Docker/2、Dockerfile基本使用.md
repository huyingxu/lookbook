## Dockerfile 基本使用

#### Dockerfile基本结构

Dockerfile 一般都是一行一个指令 组成,由`#`开头的为注解

Dockerfile主要分为四部分:

	-  基础镜像
	-  作者信息
	-  指令
	-  容器启动指令

**例如**：

```dockerfile
# 1、基础镜像部分
FROM centos

# 2、作者信息
MAINTAINER huyingxu 

# 3、指令
ADD index.html /opt

# 4、容器指令
CMD ["echo","HelloWorld"]
```

|    指令     |                             说明                             |                     语法                      |
| :---------: | :----------------------------------------------------------: | :-------------------------------------------: |
|    FROM     |              用来指定基础镜像,且必须放在第一行               |              FROM ubuntu:latest               |
| MAINTAINER  |              作者信息,用来说明镜像制作者的信息               |               MAINTAINER <name>               |
|     RUN     | 将在当前镜像之上的新图层中执行任何任命并提交结果。生成的提交镜像将用于下一步 Dockerfile |                 RUN <command>                 |
|     EVN     |                      给容器设置环境变量                      |               ENV <key> <value>               |
|             |                                                              |                                               |
|     CMD     |                     容器启动时启动的命令                     |             CMD ["run.sh","run"]              |
|   EXPOSE    |           容器运行时暴露端口给外部,启动容器上加-P            |                 EXPOSE <port>                 |
|     ADD     | 文件添加到容器中,也可以通过链接添加,COPY的升级版,可以通过URl获取资源添加到容器 |  ADD <src>... <dest>,<br> ADD <url>  <dest>   |
|    COPY     |                    将主机文件复制到容器中                    |             COPY <src>... <dest>              |
| ENTRYPOINT  |                    指定容器启动程序或参数                    |                                               |
|   VOLUME    |       实现挂功能,可将主机目录其他容器目录挂载到容器中        | VOLUME ["/data"]<br>VOLUME /data /rancherdata |
|   WORKDIR   |                  指定工作目录,可以设置多次                   |                 WORKDIR <src>                 |
|    USER     |             设定启动容器的用户,可以是用户名、UID             |                                               |
| HEALTHCHECK |                         健康检查命令                         |                                               |
|   ONBUILD   |   当当前所创建的镜像作为创建镜像的基础镜像时,所执行的命令    |            ONBUILD ADD . /app/src             |

#### 使用dockerfile创建镜像( 手动创建一个tomcat8+jdk1.8的Docker镜像)

使用docker commit虽然可以创建一个简单的镜像,但不使用我们团队的分享,这时候我们可以通过dockerfile自定义创建我们自己的镜像。首先需要创建一个dockerfile文件,然后进行写入如何创建镜像的命令。

```shell
root@localhost:~$ touch dockerfile
root@localhost:~$ vim dockerfile
```

```dockerfile
# 选择镜像，镜像+标签
FROM centos:latest
# 指定作者
MAINTAINER huyingxu 

# 将jdk添加到/var/lib/
ADD jdk1.7.0_80/ /var/lib/jdk1.7.0_80/
# 将tomcat添加到
ADD apache-tomcat-8.5.15/ /var/lib/tomcat/

# 添加环境变量
ENV JAVA_HOME /var/lib/jdk1.7.0_80
ENV CATALINA_HOME /var/lib/tomcat
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin

# 暴露8080端口
EXPOSE 8080

# 启动并运行tomcat
#CMD ["catalina.sh run", "run"]
ENTRYPOINT catalina.sh run
```

下面构建命令：

```shell
root@localhost:~$ sudo docker build -f dockerfile -t tomcat7-jdk8 .
Sending build context to Docker daemon  2.308GB
Step 1/8 : FROM centos:latest
 ---> 470671670cac
Step 2/8 : ADD jdk1.7.0_80 /var/lib/
 ---> 857e97874dc0
Step 3/8 : ADD apache-tomcat-8.5.15/ /var/lib/tomcat/
 ---> a4b916194e84
Step 4/8 : ENV JAVA_HOME /var/lib/jdk1.7.0_80
 ---> Running in 8ce62961b6e7
Removing intermediate container 8ce62961b6e7
 ---> 2ceabb5ba516
Step 5/8 : ENV CATALINA_HOME /var/lib/tomcat
 ---> Running in c80dacc7aa60
Removing intermediate container c80dacc7aa60
 ---> 19f8bd92a051
Step 6/8 : ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin
 ---> Running in 0159c4a517a5
Removing intermediate container 0159c4a517a5
 ---> 5d32401dadc8
Step 7/8 : EXPOSE 8080
 ---> Running in 8849393329ac
Removing intermediate container 8849393329ac
 ---> 65134a2ed443
Step 8/8 : ENTRYPOINT catalina.sh run
 ---> Running in 819ed1ac9de4
Removing intermediate container 819ed1ac9de4
 ---> d441a53962ce
Successfully built d441a53962ce
Successfully tagged tomcat7-jdk8:latest
root@localhost:~$ 
```

查看创建的镜像：

```shell
root@localhost:~$ docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
tomcat7-jdk8                                      latest              7fe34fce090d        13 hours ago        557MB
slera/tomcat                                      hello-world         fbc4e39e3869        14 hours ago        647MB
tomcat                                            latest              2eb5a120304e        2 days ago          647MB
root@localhost:~$ 
```

测试启动tomcat ：

```shell
root@localhost:~$ docker run -id -p 8080:8080 tomcat7-jdk8 /bin/bash
c006a438f1052fe32f63c1ed995623e2af2cf3a160ed16dc42f2bc7b8de958ff
root@localhost:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                    NAMES
c006a438f105        tomcat7-jdk8        "/bin/sh -c 'catalin…"   About a minute ago   Up About a minute   0.0.0.0:8080->8080/tcp   crazy_vaughan
root@localhost:~$ docker port c006a438f105
8080/tcp -> 0.0.0.0:8080
root@localhost:~$ 
```

浏览器打开输入http://127.0.0.1:8080,访问成功

