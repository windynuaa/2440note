# 发送格式
## 起始位
将数据线拉低并保持1bit时间
## 数据位
数据从最低位开始发送
## 校验位

## 停止位
拉高电平，时间可为1bit,1.5bit,2bit

# 数据收发

## 设置引脚
设置复用，开启上拉模式
## 设置波特率

## 设置数据格式

## 发送流程
### FIFO模式
数据传送至FIFO寄存器，移位器从FIFO中读取数据发送
### nonFIFO模式
判断发送是否完毕
数据写到URXHx寄存器
## 接收流程
### FIFO模式

### nonFIFO模式

# 实现printf