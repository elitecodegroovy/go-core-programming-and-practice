
# Go语言FQA

## Go基础问题

### 1 Go语言中，select是随机的还是顺序的？

select随机选择一个可用通用做收发操作

### 2 Go语言中的局部变量是分配在栈上吗，为什么？

Go语言编译器会自动做逃逸分析，它决定了一个变量到底是放在栈上还是放在堆上。当一个变量的作用域在函数范围时，它可以在栈上。反之，
则分配在堆上。

### 3 如何获取Go语言的协程数量、gc时间、对象数和堆栈信息？

ReadMemStats(m *MemStats)指定了内存统计M的信息。

### 4 为什么有人称呼Go语言为golang？

golang只是Go语言的一个绰号，比如，Twitter便签Go语言就是使用golang。Go语言的官方域名也是golang.org而不是go.org.

### 5 Go中那些属于函数那些属于方法

语言约定俗成的规约，公有的函数体定义称为函数，对象的私有的函数体称为方法。

### 6 调试Go程序性能的方法？

- pprof

- 火焰图(配合压测)

- 使用go run -race 或者 go build -race 来进行竞争检测

- 查看系统 磁盘IO/网络IO/内存占用/CPU 占用(配合压测)

### make和new的区别

new(T) 是为一个 T 类型的新值分配空间, 并将此空间初始化为 T 的零值, 并返回这块内存空间的地址, 也就是 T 类型的指针 *T, 该指
针指向 T 类型值占用的那块内存。 make(T) 返回的是初始化之后的 T, 且只能用于 slice, map, channel 三种类型. make(T, args)
返回初始化之后 T 类型的值, 且此新值并不是 T 类型的零值, 也不是 T 类型的指针 *T, 而是 T 类型值经过初始化之后的引用。


### 如何理解Go的runtime运行机制?

Runtime是Go语言的核心库，好比libc是C的核心库一样，也就说，runtime是提供了核心语言服务的一个库。它实现了管理任务调度，垃圾收
集及运行环境。同时，Go提供了一些高级的功能，如goroutine, channel, 以及garbage collection。这些高级功能需要一个runtime的
支持. runtime和用户编译后的代码被linker静态链接起来，形成一个可执行文件。这个文件从操作系统角度来说是一个user space的独立
的可执行文件。 从运行的角度来说，这个文件由2部分组成，一部分是用户的代码，另一部分就是runtime。runtime通过接口函数调用来管
理goroutine, channel及其他一些高级的功能。从用户代码发起的调用操作系统API的调用都会被runtime拦截并处理。

Go runtime的一个重要的组成部分是goroutine scheduler。他负责追踪，调度每个goroutine运行，实际上是从应用程序的process所属
的thread pool中分配一个thread来执行这个goroutine。因此，和java虚拟机中的Java thread和OS thread映射概念类似，每个goroutine
只有分配到一个OS thread才能运行。

### 是不是所有数据类型都可以作为map的key，为什么？
GO语言中，不是所有类型都可以作为map的key，map的key需要具备可比较性。slice、map、function类型都无法比较比较，因此，它们无法
作为map的key。例如math.NaN()返回类型是float64却是无法比较的（NaN ！= NaN），string值是NaN。

### Map是否是线程安全？
Go语言中的Map不是线程安全的。

## Go高级问题

### 1 对init函数的理解，它的加载顺序是怎么样的，用途是什么？

Go语言的init还是就是加载某个文件必须执行的一个函数，实现初始化工作，它的执行顺序是按照文件名称的字母表顺序执行的，可以理解为
bootstrap过程，完成该文件的初始化工作。init主要用途是初始化程序的执行操作。

### 2 如何理解Go垃圾回收机制？（TODO）

v1.1 STW
v1.3 Mark STW, Sweep 并行
v1.5 三色标记法
v1.8 hybrid write barrier(混合写屏障：优化STW)




### 3 Go语言的协程调度原理?（TODO）

Go语言的调度模型就是MPG，MPG可以这样定义：
M(machine): 计算资源，可以认为它是系统的系统进程。
P(processor): 逻辑processor，是进程M的执行上下文。
G(goroutine): 调度系统的最基本单位goroutine，存储了goroutine的执行stack信息、goroutine状态以及goroutine的任务函数等。


