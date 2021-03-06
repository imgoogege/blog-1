# 如何调试？

单步跟踪是最常见的调试方法，不过我用的极少，因为效率低。往往也不需要用到这个。接下来我写的几种常见的调试方式，
主要针对Go和Python。按难度从容易到难，按效率从高到低。

> 以下所有的调试的前提都是找到复现方式

- 标准输出+日志

一般日志都可以设置级别，一般都会有例如 `debug`, `info`, `error`, `fatal` 等级别。将日志输出到标准输出，在找到
复现方式之后就可以看日志，然后定位问题。不过日志调试有一个问题，如果日志打印太多，容易把真正出问题的地方掩盖掉，
如果太少了，又不够详细导致不方便调试。给日志设置不同的级别更多的是一门手艺活，凭个人品味，经验。

print也算一种，不过显然logging是更科学的方式。

- 打印调用栈

如果光看日志看不出来，可以用各种方式到异常点，看当前栈的情况，包括调用栈，变量等。比如Python中的异常，可以直接
`logging.exception` 打印，Go的panic也可以 `runtime/debug` 中的printstack打印出来。这种情况下一般配合异常收集
工具比如 `sentry` 等，可以方便快速的定位问题

- review 代码

通常结合上面的两步一起来，我个人一般到第一二步就知道问题所在了。如果还是不行，就会重读一遍代码，在大脑里模拟
处理的走向。

- 查看APM

如果有使用APM工具，可以到其dashboard看，从大局看问题所在，然后结合上面两种方式逐层细化下去。

- 单步跟踪

单步跟踪很强大，但是通常用不上。普通的调试走单步，实在是有种大炮打蚊子的感觉。常见的调试工具是pdb（Python），gdb（Go）。
