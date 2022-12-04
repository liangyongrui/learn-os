# ch7

## 简答作业

1. 举出使用 pipe 的一个实际应用的例子。
   - <font color=red>[Answer]</font>
   - rcore 书里的例子
     ```shell
     who                     # 登录Linux的用户信息
     who | grep chyyuu       # 是否用户ID为chyyuu的用户登录了
     who | grep chyyuu | wc  # chyyuu用户目前在线登录的个数
     ```
1. 如果需要在多个进程间互相通信，则需要为每一对进程建立一个管道，非常繁琐，请设计一个更易用的多进程通信机制
   - <font color=red>[Answer]</font>
   - 可以搞一个 mpmc channel

## 你对本次实验设计及难度/工作量的看法，以及有哪些需要改进的地方，欢迎畅所欲言
