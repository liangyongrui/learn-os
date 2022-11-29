# lab1

## 编程作业

1. stat: 在 DiskInode 上加上 nlink 即可
1. linkat: 创建一个文件名，但关联已有的 inode id, nlink+=1
1. unlinkat: 删除一个文件名，删除操作用和最后一个交互即可，nlink-=1, 当 nlink 为 0 时，需要 clear

### 其他

1. 各种互斥锁的关系有点乱，注意小心死锁
1. 文档里面的索引节点 指的就是 DiskInode, 而 Inode 是保存在内存上的临时结构
1. INODE_DIRECT_COUNT 需要改为 27
1. 遇到个把 opt-level = 0 注释掉就好了的错，很困惑。。

<https://github.com/LearningOS/lab4-os6-liangyongrui>

## 简答作业

1. 在我们的 easy-fs 中，root inode 起着什么作用？如果 root inode 中的内容损坏了，会发生什么？
   - <font color=red>[Answer]</font>
   - root inode 保存根目录下有哪些文件
   - 如果损坏了，文件将无法索引

## 你对本次实验设计及难度/工作量的看法，以及有哪些需要改进的地方，欢迎畅所欲言

测试用例不够完善，有些细节没考虑完（比如 unlink 没有清理），但 ci 却过了。
