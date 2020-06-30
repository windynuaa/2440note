# 点亮一个LED

## 硬件部分
* 引脚：GPF4 低电平点亮   
* 

 
### 芯片初始化
nor启动流程
> 基地址0对应nor flash。cpu从nor直接读取指令运行

nand启动流程
> 基地址0对应ram. cpu从nand中读取前4k代码复制到ram中。nor禁用

### GPIO配置
* 设置输出模式 （GPFCON 0x56000050 16bit）
  - [9:8]=00 输入
    [9:8]=01 输出
    [9:8]=10 中断
    [9:8]=11 保留
* 设置引脚数据 （GPFDAT 0x56000054 8bit）

 
### [汇编](./asm.md)程序代码
* 配置引脚
    ```c
    .text
    .global _start
    
    _start:
    //这部分为主程序
    //写0x100到控制寄存器
    ldr r1,=0x56000050
    ldr r0,=0x100
    str r0,[r1]
    //写0x10到数据寄存器
    ldr r1,=0x56000054
    ldr r0,=0x10
    str r0,[r1]
    //死循环，防止程序跑飞。
    halt:
        b halt
    ```


## 编译工具
[gcc](./gcc.md/#gcc_v)

## [Makefile](./makefile.md/)
makefile 框架
```
ARCH 	:=arm-linux-
CC   	:=gcc
LD		:=ld

all:
	arm-linux-gcc -c -o xxx.o xxx.s
	arm-linux-ld -Ttext 0 xxx.o -o xxx.elf
	arm-linux-objcopy -O binary -S xxx.elf xxx.bin

clean:
	rm *.bin
	rm *.elf
	rm *.o  
```