#ASM for ARM
# 存储指令

### LDR A,B
    将B地址的值读取到A中

### STR A,B
    将A的值写到B地址中，写入时四字节对齐，即目标地址不为4的倍数时，将地址向下取4的倍数写入，可能会破坏文件！
>例如：str r3,[0X02] 会把r3的内容写到0x00~0x03的内存空间中。 


### LDM（F\E I\D A\B） 【指针寄存器】！，{寄存器集}
    将B地址的值读取到A中，高标号的寄存器存在高地址。

### STM（F\E I\D A\B） 【指针寄存器】！，{寄存器集}
    将A的值写到B地址中，高标号的寄存器存在高地址。


> f 满栈，指向最后一个数据单元
> 
> e 空栈，指向第一个空闲单元
>  
> i 地址增
> 
> d 地址减
> 
> a 先写后加
> 
> b 先加后写
> 
> ! 访问前后会修改地址寄存器数值。用以保存当前地址
> 
> stm(ldm)ia <=> ldm(stm)db

### MOV A,B
    A=B
> #1234 立即数，长度有限
> =1235 伪指令，可为32位

# 相对跳转指令
### B label 
    跳转到指定label，为相对跳转
### Bl label 
    branch and link
    跳转到指定label,并将跳转前地址存到lr寄存器中。

# 条件跳转
### BNE LABEL (CMP A,B)
    A!=B 跳转
### BLE LABEL (CMP A,B)
    A<=B 跳转
# 算术指令
### SUB A,B,C
    A=B-C
### ADD A,B,C
    A=B+C

# 特殊指令
### MRS rx,CPSR(SPSR)
    将CPSR(SPSR)寄存器的值赋给rx
### MSR CPSR(SPSR),rx
    将rx的值赋给CPSR(SPSR)寄存器

### BIC RA,RB,#mask
将RB中，mask为1对应的位清零，存到RA寄存器中
### .ascii
    插入字符串，不自动添加结束符
### .string 
    插入字符串，自动添加结束符
### .word 
    插入一个字
# C语言与汇编

## 程序传参

* 前四个参数保存在R0~R3中
* r4~r11会被程序使用，因此需要先保存
* 返回值保存于r0中

# 汇编框架
```c

.text
.global _start
/* .global 使得连接程序（ld）能够识别 symbl
    声明symbol是全局可见的。标号_start是GNU链接器用来指定第一个要执行指令所必须的,同样的是全局可见的(并且只能出现在一个模块中)*/
//这部分为主程序
_start:
    
//死循环，防止程序跑飞。
halt:
	b halt
    

```

# 补充
汇编中标签相当于指针，指向标签对应的第一条命令