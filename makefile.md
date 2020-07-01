# Makefile
    优点:可以只编译修改的部分
    make程序在执行前会分析整个文件
    ”#“表示注释
## 命令
    @echo obj：打印obj

## 变量

### 即时变量
    a := 123
    在定义时确定
### 延时变量
    a = 123
    在使用时确定
    a ?= 123
    在使用时确定,只有第一次生效，若变量已经定义则忽略此次赋值
### 变量赋值
    a += b
## 核心规则
若不指定目标则自动生成找到的第一个目标

当<a>目标文件不存在</a>或者<a>依赖文件更新</a>，才执行命令

    目标文件: 依赖1 依赖2 .。。
    【TAB】命令

### 通配符 %
例：%.o
### 所有依赖 $^

### 第一个依赖文件 $<

## 假想目标 .PHONY: obj
    不会判断obj是否存在

## 函数
### foreach var,list,text
    #对list中每一个var变量执行text指令
    a= fst sec trd
    b= $(foreach i,$(a),$(i).o)

### filter pattern,list
    #按pattern规则找出list中符合格式的元素
    a= fst sec trd
    b= $(filter %.o,$(a))    
### filter-out pattern,list
    #按pattern规则找出list中不符合格式的元素
    a= fst.c sec.o trd.s
    b= $(filter %.c,$(a)) 
### wildcard list
    #找出list中存在的文件
    #list也可以是pattern
    a= fst.c sec.o trd.s
    b= $(wildcard $(a)) 
### patsubst pattern,replacement,list
    #找出list中符合patter的元素的部分替换为replacement
    a= fst.c sec.c trd.c
    b= $(patsubst *.c,*.o,$(a)) 

### ifneq
    ifneq (express)
        ...
    endif
### include


## CFLAG

# 模板
    #目录设置
    BUILD_DIR	:= ./build
    DEBUG_DIR	:= ./debug
    DEPENDS_DIR	:= ./dependence
    #烧录器配置
    OFLASH  := ./oflash.exe
    #编译器设置
    ARCH 	:=arm-linux-
    ARCH 	:=
    CC   	:=$(ARCH)gcc
    LD		:=$(ARCH)ld
    OBJCOPY :=$(ARCH)objcopy
    CFLAG   := -Werror -nostdlib
    #文件设置
    DST     := dev
    OBJS    := start.o
    OBJC    := led.o
    DEPENDS := $(patsubst %.o,%.d,$(OBJC))

    DEPENDS_FILES := $(patsubst %.d,$(DEPENDS_DIR)/%.d,$(DEPENDS))
    DEPENDS_FILES := $(wildcard $(DEPENDS_FILES))
    OBJ_BULIDS    := $(patsubst %.o,$(BUILD_DIR)/%.o,$(OBJS))
    OBJ_BULIDC    := $(patsubst %.o,$(BUILD_DIR)/%.o,$(OBJC))


    #主要目标
    all:$(OBJ_BULIDS) $(OBJ_BULIDC)
        $(LD) -Ttext 0 $^ -o $(BUILD_DIR)/$(DST).elf
        $(OBJCOPY) -O binary -S $(BUILD_DIR)/$(DST).elf $(DST).bin
        objdump -s $(BUILD_DIR)/$(DST).elf > $(DEBUG_DIR)/$(DST).hex
        objdump -D $(BUILD_DIR)/$(DST).elf > $(DEBUG_DIR)/$(DST).asm

    #配置环境
    config: dirs
        @echo config finished
    #建立目录
    dirs:
        mkdir $(BUILD_DIR)
        mkdir $(DEBUG_DIR)
        mkdir $(DEPENDS_DIR)
    #编译源文件
    $(BUILD_DIR)/%.o:%.s
        $(CC) $< $(CFLAG) -c -o $@ 
    $(BUILD_DIR)/%.o:%.c
        $(CC) $< $(CFLAG) -c -o $@ 
    #清理
    clean:
        rm $(BUILD_DIR)/*
        rm $(DEBUG_DIR)/* 
    clean_dir:
        rmdir $(BUILD_DIR)
        rmdir $(DEBUG_DIR)
    clean_all: clean clean_dir
        @echo clean finished