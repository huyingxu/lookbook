

## 开始使用Docker

docker 客户端非常简单 ,我们可以直接输入 docker 命令来查看到 Docker 客户端的所有命令选项。

```shell
root@localhost:~# docker
Usage:	docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/Users/six/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/Users/six/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/Users/six/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/Users/six/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit
  root@localhost:~# 
```

可以通过命令 **docker command --help** 更深入的了解指定的 Docker 命令使用方法。

例如我们要查看 **docker stats** 指令的具体使用方法：

```shell
root@localhost:~$ docker stats --help

Usage:	docker stats [OPTIONS] [CONTAINER...]

Display a live stream of container(s) resource usage statistics

Options:
  -a, --all             Show all containers (default shows just running)
      --format string   Pretty-print images using a Go template
      --no-stream       Disable streaming stats and only pull the first result
      --no-trunc        Do not truncate output
root@localhost:~$ 
```

#### 镜像搜索

```shell
root@localhost:~$ docker search ubuntu
镜像名                                                      描述                                           点赞                官方的  
NAME                                                      DESCRIPTION                                     STARS               OFFICIAL  
自动化
AUTOMATED
ubuntu                                                    Ubuntu is a Debian-based Linux operating sys…   10991               [OK]
dorowu/ubuntu-desktop-lxde-vnc                            Docker image to provide HTML5 VNC interface …   437                                     [OK]
rastasheep/ubuntu-sshd                                    Dockerized SSH service, built on top of offi…   244                                     [OK]

```

- **NAME：**镜像名
- **DESCRIPTION：**描述
- **STARS：**点赞
- **OFFICIAL：**是否官方
- **AUTOMATED：**自动化

#### 镜像拉取

根据检索出的镜像名进行拉取

```
# 默认 latest
docker pull ubuntu
# 也可以选择其他的三方镜像
docker pull darksheer/ubuntu
docker pull eclipse/ubuntu_jdk8
```

#### docker列出所有镜像

```shell
root@localhost:~$ docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
ubuntu                                            latest              1d622ef86b13        6 weeks ago         73.9MB
lu566/treesoft                                    1.0                 6ed1153f9605        21 months ago       569MB
root@localhost:~$ 
```

各个选项说明:

- **REPOSTITORY：**表示镜像的仓库源
- **TAG：**镜像的标签
- **IMAGE ID：**镜像ID
- **CREATED：**镜像创建时间
- **SIZE：**镜像大小

#### Docker 列出容器

 ```shell
root@localhost:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
8242e8629fe9        nginx:latest        "/docker-entrypoint.…"   7 seconds ago       Up 6 seconds        80/tcp              lucid_keller
root@localhost:~$ 
 ```

#### Dcoker 输出 Hello World 

```shell
root@localhost:~$ docker run ubuntu:latest /etc/echo "Hello World"
Hello World
root@localhost:~$
```

**各参数解析**

- docker  : Docker 二进制文件

- run： docker命令，与前面docker参数组成启动容器命令

- ubuntu:latest	：指定要启动的镜像和启动版本, Docker 会先在主机上查找镜像是否存在，不存在则在Docker Hub自动拉取镜像。

- /etc/echo "Hello World"	:   在启动的容器中执行命令

#### 进行交互式解析器

通过 -i 和 -t 参数 ，让容器拥有“对话”能力。



```shell
root@localhost:~$ docker run -it ubuntu:latest /bin/bash
root@33e35ff8afc0:/#
```

**各个参数解析**

- -i		允许你对容器进行标准输入进行交互
- -t  	 在新的容器指定一个伪终端、终端

#### 启动容器并后台运行

```shell
root@localhost:~$ docker run -id ubuntu:latest /bin/sh -c "echo Hello World"
eb32fbe1e9138fb0d68b5e41207c5f9e2fa4f21e11c1265240b1c9d313c36a3d
root@33e35ff8afc0:/#
```

**各个参数解析**

-d 	 保持后台运行

输出的长字符串就是容器id，对于容器来说都是唯一的

- 查看后台运行状态

  ```shell
  root@localhost:~$ docker ps
  CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
  eb32fbe1e913        ubuntu:latest       "/bin/bash"         54 seconds ago      Up 52 seconds                           tender_babbage
  root@33e35ff8afc0:/#
  ```

- 查看我们刚打印的Hello World

  ```shell
  docker logs eb32fbe1e9138fb0d68b5e41207c5f9e2fa4f21e11c1265240b1c9d313c36a3d
  # 也可简写id,只要docker能找到容器就ok
  docker logs eb32fbe1e91
  # 实时追终该容器日志  加参数 -f
  docker logs -f eb32fbe1e91
  ```

  

#### 停止容器

- 通过docker ps 查看正在运行的容器,根据CONTAINER ID选择要暂停的容器id，使用stop命令停止容器, 也可以停止所有容器docker stop $(docker ps -aq)

 ```shell
root@localhost:~$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
eb32fbe1e913        ubuntu:latest       "/bin/bash"         54 seconds ago      Up 52 seconds                           tender_babbage
root@localhost:~$ docker stop eb32fbe1e913
eb32fbe1e913
root@localhost:~$ docker stop $(docker ps -aq)
4b460ef581bf
7fbc420857af
1f3f56f1a0b5
44b20240f779
a578af8a588f
ac09133c4964
b7cd7d36a756
root@localhost:~$
 ```

#### 删除容器

  - docker ps -a 可以查看日志运行过的容器, 通过docker rm 容器id删除旧容器 , 也可以删除所有容器docker rm $(docker ps -aq)

  ```shell
#  通过docker ps 查看正在运行的容器,根据CONTAINER ID选择要暂停的容器id，使用stop命令停止容器
root@localhost:~$ docker ps -a
CONTAINER ID        IMAGE                                               COMMAND                  CREATED             STATUS                        PORTS                    NAMES
c647466ecb98        ubuntu:latest                                       "/bin/bash"              4 minutes ago       Exited (137) 4 minutes ago                             nifty_heisenberg
4b460ef581bf        ubuntu:latest                                       "/bin/sh -c 'echo He…"   10 minutes ago      Exited (0) 10 minutes ago                              thirsty_burnell
7fbc420857af        ubuntu:latest                                       "/bach/sh -c 'echo H…"   10 minutes ago      Created                                                recursing_faraday
1f3f56f1a0b5        ubuntu:latest                                       "/etc/sh -c 'echo He…"   10 minutes ago      Created                                                wizardly_germain
root@localhost:~$ docker rm c647466ecb98
c647466ecb98
root@localhost:~$ docker rm $(docker ps -aq)
4b460ef581bf
7fbc420857af
1f3f56f1a0b5
44b20240f779
a578af8a588f
ac09133c4964
b7cd7d36a756
root@localhost:~$
  ```

#### 删除镜像

- docker images 可以当前主机所有镜像, 删除镜像钱需要先停掉并删除当前镜像下所有容器,才可以将镜像删除
- 两种删除方式：一、 根据镜像id删除  二、根据镜像名+tag删除镜像，镜像名和tag之前英文符号分割

```shell
root@localhost:~$ docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
ubuntu                                            latest              1d622ef86b13        6 weeks ago         73.9MB
lu566/treesoft                                    1.0                 6ed1153f9605        21 months ago       569MB
root@localhost:~$ docker rmi 1d622ef86b13
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:747d2dbbaaee995098c9792d99bd333c6783ce56150d1b11e333bbceed5c54d7
Deleted: sha256:1d622ef86b138c7e96d4f797bf5e4baca3249f030c575b9337638594f2b63f01
Deleted: sha256:279e836b58d9996b5715e82a97b024563f2b175e86a53176846684f0717661c3
Deleted: sha256:7789f1a3d4e9258fbe5469a8d657deb6aba168d86967063e9b80ac3e1154333f
root@localhost:~$ 
root@localhost:~$ docker rmi lu566/treesoft:1.0
Untagged: lu566/treesoft:1.0
Untagged: lu566/treesoft@sha256:79b23fd78552c000a4cff58b99af9c9df870dec02dede25bd543cd2b43ba817d
Deleted: sha256:6ed1153f96056e6f40059d4e5121040fa8c4dd7b1c3827dbb04bed6a215b2890
Deleted: sha256:272147cbc57fde32508727c927fbb40cb520ed6e5def3fa0865a626409f74759
Deleted: sha256:f15aa9b68d57ec09e6993cb2f71c9b1047780297deaba54668757a67b00da72a
root@localhost:~$
```

- 当镜像下有关联容器存在,删除则报错,如下:

```shell
root@localhost:~$ docker rmi 1d622ef86b13
Error response from daemon: conflict: unable to delete 1d622ef86b13 (must be forced) - image is being used by stopped container 71efd20f835c
```

### Docker 容器连接

####   拉取一个WEB容器(Tomcat7)

我们也可以使用 **-p** 标识来指定容器端口绑定到主机端口。

两种方式的区别是:

- **-P :**是容器内部端口随机映射到主机的高端口。
- **-p :** 是容器内部端口绑定到指定的主机端口。

```
root@localhost:~$ docker search tomcat7
maluuba/tomcat7                                                                   9                                       [OK]
maluuba/tomcat7-java8             Tomcat7 with java8.                             5
bbytes/tomcat7                    Tomcat 7 image with easy war deploy from com…   3                                       [OK]
mbentley/tomcat7                                                                  2                                       [OK]
mbentley/tomcat7-oracle                                                           2                                       [OK]
zhaiweiwei/tomcat7-jdk7           CentOS7.4, JDK1.7.0_79 and Tomcat7.0.82         2
stratice/tomcat7-oracle                                                           1                                       [OK]
mminderbinder/tomcat7                                                             1                                       [OK]
root@localhost:~$ docker pull tomcat
Using default tag: latest
latest: Pulling from tomcat
85432449fd0f: Pull complete
28af0fe84646: Pull complete
829d14e4c9ca: Pull complete
56b0da1cd7f2: Pull complete
27502d76a34d: Pull complete
bb4cf2a3f72b: Pull complete
Digest: sha256:eddb408d3499bd2c18546469ac85039aea4d5e9c4dbf9725c9536cb3f13ef3ea
Status: Downloaded newer image for tomcat7:latest
docker.io/tomcat:latest
root@localhost:~$ docker run -id -p 8080:8080 tomcat /bin/bash -c "sh /usr/local/tomcat/bin/catalina.sh run"
8db92d22f80d28e3ae11fd829a4b658d0b71fc8cf154324b7922c9fe1db6c453
root@localhost:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                    NAMES
aae2772d9adc        tomcat              "/bin/bash -c 'sh /u…"   About a minute ago   Up About a minute   0.0.0.0:8080->8080/tcp   jolly_clarke
root@localhost:~$ 

```

- 也可指定容器绑定地址，比如127.0.0.1

```
root@localhost:~$ docker run -id -p 127.0.0.1:8080:8080 tomcat /bin/bash -c "sh /usr/local/tomcat/bin/catalina.sh run"
aabeb15eb669d716036d4a1b894f3aa26eb91db7e88b16332daefb8b5b197154
root@localhost:~$ 
```

- 上面例子中都是tcp端口，如要映射udp端口则需要加上/udp

```
root@localhost:~$ docker run -id -p 127.0.0.1:8080:8080/udp tomcat /bin/bash -c "sh /usr/local/tomcat/bin/catalina.sh run"
aabeb15eb669d716036d4a1b894f3aa26eb91db7e88b16332daefb8b5b197154
root@localhost:~$ 
```

**docker port** 命令可以让我们快捷地查看端口的绑定情况。

```
root@localhost:~$ docker port aabeb15eb6
8080/tcp -> 127.0.0.1:8080
```

#### 容器连接

端口映射并不是唯一把 docker 连接到另一个容器的方法。

docker有一个连接系统允许将多个容器连接在一起，共享连接信息。

docker连接会创建一个父子关系，其中父容器可以看到子容器的信息。

------

### 容器命名

当我们创建一个容器的时候，docker会自动对它进行命名。另外，我们也可以使用--name标识来命名容器，例如：

- 使用 **docker ps** 命令来查看容器名

```shell
root@localhost:~$ docker run -id -p 8080:8080 --name tomcat-test  tomcat /bin/bash -c "sh /usr/local/tomcat/bin/catalina.sh run"
aabeb15eb669d716036d4a1b894f3aa26eb91db7e88b16332daefb8b5b197154
root@localhost:~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
3a6a62d7006e        tomcat              "/bin/bash -c 'sh /u…"   5 seconds ago       Up 4 seconds        0.0.0.0:8080->8080/tcp   tomcat-test
```

#### Docker 创建镜像

##### 创建已有镜像

先启动一个容器,记住容器id

```shell
root@localhost:~$ docker run -id -p 127.0.0.1:8080:8080/udp tomcat /bin/bash -c "sh /usr/local/tomcat/bin/catalina.sh run"
aabeb15eb669d716036d4a1b894f3aa26eb91db7e88b16332daefb8b5b197154
root@localhost:~$ 
```

创建首页,Hello World输入到index.html中,并均具容器id将index.html拷贝到容器指定目录下

 ```shell
root@localhost:~$ docker run -id -p 8080:8080  tomcat /bin/bash -c "sh /usr/local/tomcat/bin/catalina.sh run"
1d70c39750ebcbe689a65edea1938de88e5f1c3fcf8cb8a61dc257ebe7b35846
root@localhost:~$ echo "Hello World" > index.html
root@localhost:~$ docker cp index.html 1d70c39750eb:/usr/local/tomcat/webapps/ROOT
root@localhost:~$ 
 ```

通过http://127.0.0.1:8080/访问可以看到Hello World

然后我们将已经修改的容器通过commit命令进行提交

```shell
root@localhost:~$ docker commit -a "huyingxu" -m "set Hello World as home page" 1d70c39750eb slera/tomcat:hello-world
sha256:fbc4e39e3869d120d1beb365f8ceeb1723ad405a9c5f6a76b92583fa15443e0a
root@localhost:~$ docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
slera/tomcat                                      hello-world         fbc4e39e3869        19 seconds ago      647MB
tomcat                                            latest              2eb5a120304e        41 hours ago        647MB
root@localhost:~$ 
```

然后我们可以使用新的容器进行启动

```shell
root@localhost:~$ docker run -id -p 8080:8080 slera/tomcat:hello-world /bin/bash -c "sh /usr/local/tomcat/bin/catalina.sh run"
157ea6bd2ff4ac78dfefc5ffe1de4263ff0546bc5e509bd8392e5fd826461fcc
root@localhost:~$ 
```

通过http://127.0.0.1:8080/就可以看到我们修改的内容

