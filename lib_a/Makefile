# @Author: fh
# @FileName: Makefile
# @Date:   2018-02-24 10:01:21
# @Last Modified by:   fh
# @Last Modified time: 2018-02-24 10:01:21
# @Email:  feng.haohao@corget.com


# 目录结构
# TOP_DIR := $(shell pwd)
TOP_DIR := .
DIR_SRC := src
DIR_OBJ := obj
DIR_BIN := bin
DIR_LIB := lib

#compile and lib parameter
#编译参数
CC      := gcc
CXX     := g++
AR      := ar

RANLIB  := ranlib
LIBS    := -lpthread
LDFLAGS := -L./lib/
DEFINES :=
INCLUDE := -I./include
CFLAGS  := -g -Wall -O3 -std=c++11 $(DEFINES) $(INCLUDE)
CXXFLAGS:= $(CFLAGS) -DHAVE_CONFIG_H

SRC_DIRS    := $(shell find $(TOP_DIR) -maxdepth 4 -type d)

#共享库文件名，lib*.a
TARGET      := $(TOP_DIR)/$(DIR_LIB)/libtest.a

CXX_SRCS    = $(foreach dir, $(SRC_DIRS), $(wildcard $(dir)/*.cpp))
CXX_OBJS    = $(patsubst %.cpp, %.o, $(CXX_SRCS))

C_SRCS  = $(foreach dir, $(SRC_DIRS), $(wildcard $(dir)/*.c))
C_OBJS  = $(patsubst %.c, %.o, $(C_SRCS))


all:$(TARGET)
#根据类型生成依赖进而编译生成.o文件，并且存放到DIR_OBJ目录下
$(TARGET):$(CXX_OBJS) $(C_OBJS)
	$(shell if [ ! -d $(TOP_DIR)/lib ]; then mkdir $(TOP_DIR)/lib; fi)
	$(AR) cru $@ $(DIR_OBJ)/*.o
	$(RANLIB) $(TARGET)
	@echo ---- finished ----

%.o:%.cpp
	$(shell if [ ! -d $(TOP_DIR)/$(DIR_OBJ) ]; then mkdir $(TOP_DIR)/$(DIR_OBJ); fi)
	$(CXX) $(CXXFLAGS) -c -o $(DIR_OBJ)/$(notdir $@) $<
%.o:%.c
	$(shell if [ ! -d $(TOP_DIR)/$(DIR_OBJ) ]; then mkdir $(TOP_DIR)/$(DIR_OBJ); fi)
	$(CC) $(CXXFLAGS) -c -o $(DIR_OBJ)/$(notdir $@) $<

.PHONY:clean
clean:
	rm -rf $(TARGET) $(DIR_OBJ)/*.o
	@echo ---- clean ----

