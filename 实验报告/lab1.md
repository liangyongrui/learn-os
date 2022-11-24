# lab1

## 编程作业

<https://github.com/LearningOS/lab1-os3-liangyongrui>

1. 注意栈溢出问题
1. 注意开始时间点的设置时机

## 简答作业

1. 正确进入 U 态后，程序的特征还应有：使用 S 态特权指令，访问 S 态寄存器后会报错。 请同学们可以自行测试这些内容 (运行 Rust 三个 bad 测例 (ch2b*bad*\*.rs) ， 注意在编译时至少需要指定 LOG=ERROR 才能观察到内核的报错信息) ， 描述程序出错行为，同时注意注明你使用的 sbi 及其版本。

   - <font color=red>[Answer]</font>
   - `make test2 LOG=ERROR`

   - 执行结果

     ```text
     bad address
     [ERROR] [kernel] PageFault in application, core dumped.
     bad instructions
     [ERROR] [kernel] IllegalInstruction in application, core dumped.
     bad register
     [ERROR] [kernel] IllegalInstruction in application, core dumped.
     ```

2. 深入理解 trap.S 中两个函数 `__alltraps` 和 `__restore` 的作用，并回答如下问题:

   1. L40：刚进入 `__restore` 时，a0 代表了什么值。请指出 `__restore` 的两种使用情景。

      - <font color=red>[Answer]</font>
      - a0 表示 trap 上下文的地址
      - 两种使用情景
        - 开始运行一个应用
        - 处理完 trap 后，返回用户态

   1. L46-L51：这几行汇编代码特殊处理了哪些寄存器？这些寄存器的的值对于进入用户态有何意义？请分别解释。

      ```asm
      ld t0, 32*8(sp)
      ld t1, 33*8(sp)
      ld t2, 2*8(sp)
      csrw sstatus, t0
      csrw sepc, t1
      csrw sscratch, t2
      ```

      - <font color=red>[Answer]</font>
      - sstatus 发生异常时的处理器状态
      - sepc 发生异常或中断的时候，将要执行但未成功执行的指令地址
      - sscratch 用于交互栈指针

   1. L53-L59：为何跳过了 x2 和 x4？

      ```asm
      ld x1, 1*8(sp)
      ld x3, 3*8(sp)
      .set n, 5
      .rept 27
      LOAD_GP %n
      .set n, n+1
      .endr
      ```

      - <font color=red>[Answer]</font>
      - x2 是 sp 本身
      - x4 用不到

   1. L63：该指令之后，sp 和 sscratch 中的值分别有什么意义？

      ```asm
      csrrw sp, sscratch, sp
      ```

      - 把 trap 上下文保存到 sscratch
      - 恢复原来的 sp

   1. `__restore`：中发生状态切换在哪一条指令？为何该指令执行之后会进入用户态？

      - <font color=red>[Answer]</font>
      - 最后一行 sret
      - sret 指令具体完成以下功能：
        - CPU 会将当前的特权级按照 sstatus 的 SPP 字段设置为 U 或者 S ；
        - CPU 会跳转到 sepc 寄存器指向的那条指令，然后继续执行。

   1. L13：该指令之后，sp 和 sscratch 中的值分别有什么意义？

      ```asm
      csrrw sp, sscratch, sp
      ```

      - <font color=red>[Answer]</font>
      - 把 sp 保存到 sscratch 中
      - sscratch 之前保存的 trap 上下文地址，放入 sp 中

   1. 从 U 态进入 S 态是哪一条指令发生的？
      - <font color=red>[Answer]</font>
      - ecall 、异常指令、中断等

## 你对本次实验设计及难度/工作量的看法，以及有哪些需要改进的地方，欢迎畅所欲言

简答作业放在第二章内容下面，其实更合适
