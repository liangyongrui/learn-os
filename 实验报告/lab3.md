# lab1

## 编程作业

1. spawn
   - 其实还是 fork 和 exec的组合
   - 但不需要复制地址空间
   - 需要注意的是 CI 的原理是用 ch5_usertest 替代 ch5b_initproc ，使内核在所有测例执行完后直接退出。这个坑了我好久，在纠结为什么panic了
1. stride
   - 模拟就好了，注意返回值

<https://github.com/LearningOS/lab3-os5-liangyongrui>

## 简答作业

1. stride 算法原理非常简单，但是有一个比较大的问题。例如两个 stride = 10 的进程，使用 8bit 无符号整形储存 pass， p1.pass = 255, p2.pass = 250，在 p2 执行一个时间片后，理论上下一次应该 p1 执行。

   - 实际情况是轮到 p1 执行吗？为什么？
     - <font color=red>[Answer]</font>
     - 数字溢出了，所以要p2执行
1. 我们之前要求进程优先级 >= 2 其实就是为了解决这个问题。可以证明， 在不考虑溢出的情况下 , 在进程优先级全部 >= 2 的情况下，如果严格按照算法执行，那么 PASS_MAX – PASS_MIN <= BigStride / 2。
   - 为什么？尝试简单说明（不要求严格证明）。
     - <font color=red>[Answer]</font>
     - 因为每次都是小的先增加，所以拉开的距离不会大于BigStride / 2
1. 已知以上结论，考虑溢出的情况下，可以为 pass 设计特别的比较器，让 BinaryHeap<Pass> 的 pop 方法能返回真正最小的 Pass。补全下列代码中的 partial_cmp 函数，假设两个 Pass 永远不会相等。
   - <font color=red>[Answer]</font>
   - 如果大于二倍就代表有溢出了
     ```rust
     use core::cmp::Ordering;

     struct Pass(u64);

     impl PartialOrd for Pass {
         fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
             if (self.0 < other.0 && u64::MAX / 2 > other.0 - self.0)
                 || (self.0 > other.0 && self.0 - other.0 > u64::MAX / 2)
             {
                 Some(Ordering::Less)
             } else if self.0 == other.0 {
                 Some(Ordering::Equal)
             } else {
                 Some(Ordering::Greater)
             }
         }
     }

     impl PartialEq for Pass {
         fn eq(&self, other: &Self) -> bool {
             false
         }
     }
     ```

    TIPS: 使用 8 bits 存储 pass, BigStride = 255, 则: (125 < 255) == false, (129 < 255) == true.


## 你对本次实验设计及难度/工作量的看法，以及有哪些需要改进的地方，欢迎畅所欲言
