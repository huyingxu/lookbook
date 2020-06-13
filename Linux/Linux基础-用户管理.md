## 用户管理

#### 创建用户

- adduser 用户名
- useradd 用户名

**adduser和useradd的区别？**

 - Centos：

   在CentOs中是没有区别的,都是创建用户,在/home目录下自动创建用户目录,没有设置密码,需要使用passwd进来设置密码 ( 修改后才可以登录)

 - Ubuntu：

   useradd在使用该命令创建用户时是不会在/home目录下创建与用户名相同的用户目录,且不会自动选择shell版本,也没有密码.需要使用passwd进行设置密码

   adducer在使用该命令创建用户时会在/home目录下创建与用户名相同的用户目录,会自动选择shell版本, 在用户登录时会提示设置用户密码。

#### 删除用户

1）只删除用户

​		 userdel 用户名

2）删除用户的同时,也删除/home目录下的所对应的用户目录一同删除

​		userdel -r 用户名

 注：如果当用户创建时,与之相同的用户目录已经存在时, 即目录不属于当前要删除的用户,则用户目录会删除失败

#### 用户切换 和 退出

1）切换用户(su)（switch user的缩写,代表用户切换,使用 su切换权限, su 后面加个“-” ,切换到当前用户时,工作目录会发生变化

```shell
slera@localhost:~$ pwd
/opt/nginx/html
slera@localhost:~$ su
password:
root@localhost:~$ pwd
/opt/nginx/html
```

当su后面追加“-”时：

```shell
slera@localhost:~$ pwd
/opt/nginx/html
slera@localhost:~$ su -
password:
root@localhost:~$ pwd
/root
root@localhost:~$ 
```

当su后面追加“-”时：

在文件中找到如下以下一行：

2 ）退出系统 exit

​	   root@localhost:~$ exit

说明：退出方法有很多种,exit是最安全的

#### 赋予管理员权限

首先,获取root权限,编辑/etc/sudoers文件

```
slera@localhost:~$ su root
root@localhost:~$ vim /etc/sudoers
```

在文件中找到如下以下一行：

```
root    ALL=(ALL)       ALL
```

然后再下面追加一行,然后保存并退出

用户   ALL=(ALL)       ALL