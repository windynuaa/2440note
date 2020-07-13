nand flash
# 信号类型
## ALE Address Lock Enable
高电平地址，低电平数据

## CLE Command Lock Enable
高电平命令，低电平数据

## RnB 
高电平就绪，低电平忙


# 操作流程
## 配置控制器：
    NFCONF
## 发命令
    NFCMMD
## 发地址
    NFADDR
## 读写数据
    NFDATA
    读写数据时一般以页（一整行）为单位
    擦除时一般以块（很多页）为单位