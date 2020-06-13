### 命令基础

- Ctrl + z和Ctrl + c区别

  两者都是中断命令，但是两者应用场景不一样

  Ctrl + c 是强制终端命令

  Ctrl + z是将任务终端,但是任务并没有结束,他的进程处于挂起状态

  

  按下Ctrl + c 时：

  ```shell
  root@localhost:~$ ping www.baidu.com
  PING www.a.shifen.com (14.215.177.38): 56 data bytes
  64 bytes from 14.215.177.38: icmp_seq=0 ttl=63 time=2.897 ms
  64 bytes from 14.215.177.38: icmp_seq=1 ttl=63 time=3.148 ms
  ^C
  --- www.a.shifen.com ping statistics ---
  2 packets transmitted, 2 packets received, 0.0% packet loss
  round-trip min/avg/max/stddev = 2.897/3.022/3.148/0.125 ms
  root@localhost:~$ 
  ```

  按下Ctrl + z 时：

  ```shell
  root@localhost:~$ ping www.baidu.com
  PING www.a.shifen.com (14.215.177.38): 56 data bytes
  64 bytes from 14.215.177.38: icmp_seq=0 ttl=63 time=3.287 ms
  64 bytes from 14.215.177.38: icmp_seq=1 ttl=63 time=2.606 ms
  ^Z
  [1]  + 6930 suspended  ping www.baidu.com
  root@localhost:~$ 
  ```

  可以通过jobs查看已挂载的程序,不用携带任何参数,也可使用ps查看到挂载的进程

  ```shell
  root@localhost:~$ jobs
  [1]  - suspended  ping www.baidu.com
  [2]  + suspended  ping www.qq.com
  root@localhost:~$ ps
  17888 pts/1    00:00:00 bash
  17959 pts/1    00:00:00 ping
  17960 pts/1    00:00:00 ping
  17987 pts/1    00:00:00 ping
  ```

  当按下Ctrl + z 后,用户可以通过fg/bg操作前台任务或后台任务

  - bg  将后台停止的进程在后台继续运行, 用法：bg %num
  - fg   将后台停止的进程调至前台运行, 用法：fg %num

  ```shell
  root@localhost:~$ fg
  [1]  + 6930 continued  ping www.baidu.com
  64 bytes from 14.215.177.38: icmp_seq=2 ttl=63 time=3.543 ms
  64 bytes from 14.215.177.38: icmp_seq=3 ttl=63 time=3.003 ms
  64 bytes from 14.215.177.38: icmp_seq=4 ttl=63 time=3.105 ms
  64 bytes from 14.215.177.38: icmp_seq=5 ttl=63 time=3.363 ms
  ...
  ```

  如果存在多个job, 可通过job编号继续运行

  ```shell
  [root@localhost ~]# fg 2
  ping www.baidu.com
  64 bytes from 14.215.177.38 (14.215.177.38): icmp_seq=3 ttl=56 time=2.97 ms
  64 bytes from 14.215.177.38 (14.215.177.38): icmp_seq=4 ttl=56 time=2.72 ms
  ...
  ```

  使用bg命令

  ```shell
  [root@localhost ~]# bg %3
  [3]+ ping www.baid.com &
  [root@localhost ~]# bg 3
  [3]+ ping www.baid.com &
  [root@localhost ~]# 
  ```

  

