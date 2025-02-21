# Makefile 模板

# 项目信息
PROJECT_NAME = MyProject

# 编译器和编译选项
CXX = g++
CXXFLAGS = -std=c++17 -Wall -Wextra -g

# 目标类型：可执行文件、静态库或共享库
BUILD_EXECUTABLE = 1
BUILD_STATIC_LIB = 0
BUILD_SHARED_LIB = 0

# 文件路径
SRC_DIR = src
INCLUDE_DIR = include
LIB_DIR = lib
BIN_DIR = bin
OBJ_DIR = obj

# 文件收集
SOURCES = $(wildcard $(SRC_DIR)/*.cpp)
OBJECTS = $(patsubst $(SRC_DIR)/%.cpp,$(OBJ_DIR)/%.o,$(SOURCES))
HEADERS = $(wildcard $(INCLUDE_DIR)/*.h)

# 输出目录
OUTPUT_DIR = $(BIN_DIR)
STATIC_LIB_OUTPUT = $(LIB_DIR)/lib$(PROJECT_NAME).a
SHARED_LIB_OUTPUT = $(LIB_DIR)/lib$(PROJECT_NAME).so

# 默认目标
.PHONY: all
all: $(OUTPUT_DIR)/$(PROJECT_NAME)

# 可执行文件构建规则
$(OUTPUT_DIR)/$(PROJECT_NAME): $(OBJECTS)
	$(CXX) $(OBJECTS) -o $@ $(LDFLAGS)

# 静态库构建规则
$(STATIC_LIB_OUTPUT): $(OBJECTS)
	ar rcs $@ $(OBJECTS)

# 共享库构建规则
$(SHARED_LIB_OUTPUT): $(OBJECTS)
	$(CXX) -shared $(OBJECTS) -o $@

# 编译源文件为目标文件
$(OBJ_DIR)/%.o: $(SRC_DIR)/%.cpp $(HEADERS)
	$(CXX) $(CXXFLAGS) -I$(INCLUDE_DIR) -c $< -o $@

# 清理目标文件
.PHONY: clean
clean:
	rm -f $(OBJECTS) $(STATIC_LIB_OUTPUT) $(SHARED_LIB_OUTPUT) $(OUTPUT_DIR)/$(PROJECT_NAME)

# 安装规则（可选）
.PHONY: install
install: $(OUTPUT_DIR)/$(PROJECT_NAME)
	install -m 755 $(OUTPUT_DIR)/$(PROJECT_NAME) /usr/local/bin

# 可选的调试/发布模式设置
ifeq ($(BUILD_EXECUTABLE), 1)
	CXXFLAGS += -O2
	LDFLAGS = 
endif

ifeq ($(BUILD_STATIC_LIB), 1)
	LDFLAGS = -static
endif

ifeq ($(BUILD_SHARED_LIB), 1)
	LDFLAGS = -shared
endif
