## chmod用法

**chmod命令主要用户修改用户修改设置文件的权限**

#### 1.语法(字母赋值)

```shell
chmod [ugoa...][[+-=][rwxX]...][,...]
```

#### 通过字母修改用户对文件的操作权限

**用法：chmod  [u,g,o,a] [+,-,=] [r,w,x] 文件名**

| [u,g,o,a] |                  含义                  |
| :-------: | :------------------------------------: |
|     u     |           user表示文件所有者           |
|     g     | group表示与该文件的所有同一组,即用户组 |
|     o     |          other表示其他用户组           |
|     a     |        All代表所有权限（默认）         |

|  -   |   含义   |
| :--: | :------: |
|  +   | 赋予权限 |
|  -   | 撤销权限 |
|  =   | 设定权限 |

|  -   |                             含义                             |
| :--: | :----------------------------------------------------------: |
|  r   | read表示可以读取,对于一个目录,如果没有r权限则当前目录下无法使用ls查看这个目录的内容 |
|  w   | write 表示写,对于一个目录,如果没有w权限则当前目录下无法创建或修改文件 |
|  x   | excute表示可执行对于一个目录,如果没有x目录,则该用户无法cd到这个目录下 |
|  x   | X 表示只有当该文件是个子目录或者该文件已经被设定过为可执行。 |

**举个栗子：**

```shell
root@localhost:~$ chmod g+rw index.html
root@localhost:~$ ll
total 1020
-rwxrw---- 1 root   root         0 Jun 12 15:28 index.html
root@localhost:~$ 
```

说明 ： 为index.html文件赋予用户组读写权限,原有权限不变,如果用户类型已有权限使用+不受影响

```shell
root@localhost:~$ chmod g=rw index.html
root@localhost:~$ ll
total 1020
-rwxrw---- 1 root   root         0 Jun 12 15:28 index.html
root@localhost:~$ 
```

说明 ： 为index.html文件设置为用户组读写权限,清除旧权限,使用新设置的

```shell
root@localhost:~$ chmod u+rwx,g+rwx,o-rw index.html
root@localhost:~$ ll
total 1020
-rwxrwx--- 1 root   root         0 Jun 12 15:28 index.html
root@localhost:~$ 
```

 说明：修改index.html 的读写权限,所有者(user)拥有可读(read)可写(write)可执行(excute)权限,所有者拥有可读(read)可写(write)可执行(excute)权限,用户组拥有可读(read)可写(write)可执行(excute)权限,其他组撤销可读(read)可写(write)权限

#### 2.语法(数字赋值)

根据字母赋值做了简化, 用法：chmod + 三位数字组合 + 文件名



| 三位数子组合    |
| --------------- |
| 第一位数字表示u |
| 第二位数字表示g |
| 第三位数字表示o |

 

| 权限 | 数字表示 |
| :--: | :------: |
|  r   |    4     |
|  w   |    2     |
|  x   |    1     |

- 若要rwx属性则4+2+1=7；
- 若要rw-属性则4+2=6；
- 若要r-x属性则4+1=5。



**举个栗子**

```shell
root@localhost:~$ chmod 774 inex.html
root@localhost:~$ ll
total 1020
-rwxrwxr-- 1 root   root         0 Jun 12 15:28 index.html
root@localhost:~$ 
```

说明：第一个7代表所有者所赋予的权限读(4)写(2)执行(1)权限,第二个7代表用户组权赋予限读(4)写(2)执行(1)权限,其他赋予读(4)的权限

**与以下命令效果相同**

```
root@localhost:~$ chmod u=wrx,g=rwx,o=r index.html
root@localhost:~$ ll
total 1020
-rwxrwxr-- 1 root   root         0 Jun 12 15:28 index.html
root@localhost:~$ 
```

#### 附录 —— chmod 数字法另一种解释：

上面的解释是基于求和的方法，下面用二进制的方法进行解释数字法表示：
r  w  x 权限用用三位二进制数字表示：
第一位数字（0或1）表示 r, 为1表示有效， 0无效
第二位数字 （0或1) 表示 w, 为1表示有效，0无效
第三位数字 （0或1) 表示 x, 为1表示有效， 0无效
000  <--------------------->    无任何权限
100  <--------------------->    r(read)      <-----> 4
010  <--------------------->    w(write)    <-----> 2
001 <---------------------->    x(excute)  <---->  1
101 <---------------------->    rx              <---->  5
110 <---------------------->    rw             <---->  6
111 <---------------------->    rwx           <-----> 7

————————————————
原文链接：https://blog.csdn.net/lyy14011305/article/details/76333041

