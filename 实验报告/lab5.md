# lab5

## 编程作业

就是实现银行家算法。

不过因为 mutex 只有 true/false, 所以情况可以简化

semaphore 需要注意内存的提前申请，避免数组下标越界

<https://github.com/LearningOS/lab5-os8-liangyongrui>

## 简答作业

1. 在我们的多线程实现中，当主线程 (即 0 号线程) 退出时，视为整个进程退出， 此时需要结束该进程管理的所有线程并回收其资源。

   - 需要回收的资源有哪些？
     - <font color=red>[Answer]</font>
     - 页表、文件
     - 子线程的 TaskUserRes（这里不能用 rust 的所有权自动回收，避免后面收到回收页表的时候回收了两次）
     - 子进程挂到 root 上
   - 其他线程的 TaskControlBlock 可能在哪些位置被引用，分别是否需要回收，为什么？
     - <font color=red>[Answer]</font>
     - 所有用到的地方，比如 mutex、semaphore 相关的操作上
     - 不用，有 rust 的所有权自己管理就好了

2. 对比以下两种 Mutex.unlock 的实现，二者有什么区别？这些区别可能会导致什么问题？

   ```rust
   impl Mutex for Mutex1 {
      fn unlock(&self) {
         let mut mutex_inner = self.inner.exclusive_access();
         assert!(mutex_inner.locked);
         mutex_inner.locked = false;
         if let Some(waking_task) = mutex_inner.wait_queue.pop_front() {
            add_task(waking_task);
         }
      }
   }

   impl Mutex for Mutex2 {
      fn unlock(&self) {
         let mut mutex_inner = self.inner.exclusive_access();
         assert!(mutex_inner.locked);
         if let Some(waking_task) = mutex_inner.wait_queue.pop_front() {
            add_task(waking_task);
         } else {
            mutex_inner.locked = false;
         }
      }
   }
   ```

   - <font color=red>[Answer]</font>
   - 第二种明显错的，唤醒了一个，却不解锁

## 你对本次实验设计及难度/工作量的看法，以及有哪些需要改进的地方，欢迎畅所欲言

最后一题，感觉难度略小。甚至不如前两个 lab 细节多
