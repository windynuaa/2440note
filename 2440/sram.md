# 访存流程
cpu只能访问soc内部地址。
cpu发送地址给内存控制器，内存控制器根据地址决定如何读写（使用内存接口）外部内存芯片：发送片选信号

## 内存接口-地址信息可以传送至外部
    1、网卡
    2、norflash

## 片选引脚
    内存控制器根据不同的地址区段，发出不同的片选信号（每个片段128m）

# 时序配置