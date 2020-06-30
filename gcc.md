# <a name="gcc_v">gcc 不同版本区别</a>

## 1 arm-none-eabi-gcc
Arm官方用于编译 ARM 架构的裸机系统（包括 ARM Linux 的 boot、kernel，使用[newlib](https://baike.baidu.com/item/newlib/1886687?fr=aladdin)库，不适用编译 Linux应用），一般适合 ARM7、Cortex-M 和 Cortex-R 内核的芯片使用。
## 2 arm-none-linux-gnueabi-gcc
可用于编译 ARM 架构的 u-boot、Linux内核、Linux 应用等。arm-none-linux-gnueabi基于GCC，使用Glibc库，经过 Codesourcery 公司优化过推出的编译器。arm-none-linux-gnueabi-xxx 交叉编译工具的浮点运算非常优秀。一般ARM9、ARM11、Cortex-A 内核，带有 Linux 操作系统的会用到。
## 3 arm-eabi-gcc
Android ARM 编译器
## 4 abi and eabi
Application Binary Interface (ABI) for the ARM Architecture.
> 在armel中，关于浮点数计算的约定有三种。以gcc为例，对应的-mfloat-abi参数值有三个：soft,softfp,hard。soft是指所有浮点运算全部在软件层实现，效率当然不高，适合于早期没有浮点计算单元的ARM处理器；softfp是目前armel的默认设置，它将浮点计算交给FPU处理，但函数参数的传递使用通用的整型寄存器而不是FPU寄存器；hard则使用FPU浮点寄存器将函数参数传递给FPU处理。
需要注意的是，在兼容性上，soft与后两者是兼容的，但softfp和hard两种模式不兼容。默认情况下，armel使用softfp，因此将hard模式的armel单独作为一个abi，称之为armhf。

使用softfp模式，会存在不必要的浮点到整数、整数到浮点的转换。而使用hard模式，在每次浮点相关函数调用时，平均能节省20个CPU周期。对ARM这样每个周期都很重要的体系结构来说，这样的提升无疑是巨大的。

在完全不改变源码和配置的情况下，在一些应用程序上，使用armhf能得到20——25%的性能提升。对一些严重依赖于浮点运算的程序，更是可以达到300%的性能提升。
