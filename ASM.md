#ASM for ARM

### LDR A,B
    将B地址的值读取到A中

### STR A,B
    将A的值写到B地址中

### MOV A,B
    A=B
> #1234 立即数，长度有限
> =1235 伪指令，可为32位

### B label
    跳转到指定label，为相对跳转
### Bl label
    跳转到指定label,并将跳转前地址存到lr寄存器中。
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