# @Author: fh
# @FileName: Makefile
# @Date:   2018-02-24 10:01:21
# @Last Modified by:   fh
# @Last Modified time: 2018-02-24 10:01:21
# @Email:  feng.haohao@corget.com


# 目录结构
# TOP_DIR := $(shell pwd)
TOP_DIR := .
DIR_SRC := $(TOP_DIR)/src
DIR_OBJ := $(TOP_DIR)/obj
DIR_BIN := $(TOP_DIR)/bin

CC := g++
LD := g++

CFLAGS     += -Wall -O2 -std=c++11 -I./include
LDFLAGS    += -lpthread

# 定义可执行文件生成的目录
# ?=是如果没有被赋值过就赋予等号后面的值
BINARY_PATH ?= $(DIR_BIN)

# 定义obj文件生成的目录
OBJECT_PATH ?= $(DIR_OBJ)

#最终目标文件
TARGET 		?= $(BINARY_PATH)/my-app


# 获取当前目录,src及以下若干子目录的cpp源文件，并赋值给SOURCES
SRC_DIRS    := $(shell find $(TOP_DIR) -maxdepth 4 -type d)
C_SRCS  = $(foreach dir, $(SRC_DIRS), $(wildcard $(dir)/*.c))
CXX_SRCS    = $(foreach dir, $(SRC_DIRS), $(wildcard $(dir)/*.cpp))
# C_OBJS  = $(patsubst %.c, %.o, $(C_SRCS))
# CXX_OBJS    = $(patsubst %.cpp, %.o, $(CXX_SRCS))
SOURCES := $(C_SRCS) $(CXX_SRCS)

# 1.取得SOURCES源文件中不包含路径和后缀的文件名;
# 2.为文件名添加路径；
# 3.为文件名添加.o后缀
OBJECTS := $(addsuffix .o,$(addprefix $(OBJECT_PATH)/,$(basename $(notdir $(SOURCES)))))

# 为obj文件添加.d后缀
DEPENDS := $(addsuffix .d,$(OBJECTS))

# ------ 以下不明白，好像是控制hello和post-build的是输出------
# ------ 不明白hide?=@ 中@是是什么意思------
# ------ 不明白变量$(v)，这个v什么时候能赋值------
ifneq (v$(v),v1)
hide?=@
else
hide?=
endif

# 定义伪目标
.PHONY : all clean hello prepare build post-build

all: hello prepare build post-build
clean: ;rm -rf $(TARGET) $(OBJECT_PATH)

# 输出 信息
hello: ;$(hide) echo ==== start, $(shell date) ====
prepare: $(BINARY_PATH) $(OBJECT_PATH)
post-build: ;$(hide) echo ==== done,  $(shell date) ====

# 创建目录
build: $(TARGET)
$(BINARY_PATH) $(OBJECT_PATH): ; mkdir -p $@

# 生成目标可执行文件
$(TARGET) : $(OBJECTS)
	$(info $(LD) $@)
	$(hide) $(LD) -o $@ $(OBJECTS) $(LDFLAGS)

# 定义命令序列
# ------ 不明白为何这些makefile函数前面都使用了两个$$------
define make-cmd-cc
$2 : $1
	$$(info $(CC) $$<)
	$$(hide) $$(CC) $$(CFLAGS) -MMD -MT $$@ -MF $$@.d -c -o $$@ $$<
endef

# $(eval text), text 的内容将作为makefile的一部分而被make解析和执行。
$(foreach afile,$(SOURCES),\
	$(eval $(call make-cmd-cc,$(afile),\
		$(addsuffix .o,$(addprefix $(OBJECT_PATH)/,$(basename $(notdir $(afile))))))))

-include $(DEPENDS)
