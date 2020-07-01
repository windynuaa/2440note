# 2440 时钟体系

## CPU内部时钟
PLL=FCLK 400Mhz

## AHB总线 高速总线
PLL->HDIV=HCLK 136Mhz

## APB总线 低速外设总线
PLL->PDIV=PCLK 68Mhz

## 时钟源选择
使用OM[3:2]引脚控制，软件改变PLL时，CPU会停止一段时间

# 锁相环

## PLLCON 寄存器
### MPLLCON 主PLL控制 0x4c000004
    //in 400 Mhz
    MDIV MPLLCON[19:12] =0x5c
    PDIV MPLLCON[9:4]   =0x1
    SDIV MPLLCON[1:0]   =0x1

    m=MDIV+8
    p=PDIV+2
    s=SDIV

    Fclk=2*m*fin/(p*2^s)
### UPLLCON USBPLL控制
    
## CLKCON 寄存器
    配置不同模块的时钟，用以省电

## CLKDIVN 0x4c000014
配置分频系数,改变hclk与fclk、pclk与hclk之间的关系.

    DIVN_UPLL
    HDIVN
    PDIVN
> 若HDIVN不等于0则需开启异步模式

    MMU_SetAsyncBusMode
    mrc p15,0,r0,c1,c0,0
    orr r0,r0,#0xc0000000
    mcr p15,0,r0,c1,c0,0

## 配置流程
设置locktime->
设置分频系数->
设置异步模式->
设置PLL系数
# 关闭看门狗
    0x53000000=0

