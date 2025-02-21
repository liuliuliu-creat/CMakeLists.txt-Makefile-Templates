# 设置 CMake 最低版本要求
cmake_minimum_required(VERSION 3.10)

# 设置项目名称和版本
project(MyProject VERSION 1.0 LANGUAGES CXX)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# 设置可执行文件和库文件的输出目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)  # 静态库的输出目录

# 构建选项
option(BUILD_EXECUTABLE "Build the project as an executable" ON)
option(BUILD_STATIC_LIB "Build the project as a static library" OFF)
option(BUILD_SHARED_LIB "Build the project as a shared library" OFF)

# 处理跨平台的编译选项（如需要）
if(WIN32)
    message(STATUS "Building on Windows")
    # 设置 Windows 特定的编译选项
elseif(APPLE)
    message(STATUS "Building on macOS")
    # 设置 macOS 特定的编译选项
else()
    message(STATUS "Building on Linux")
    # 设置 Linux 特定的编译选项
endif()

# 收集源文件和头文件
file(GLOB SOURCES "src/*.cpp")
file(GLOB HEADERS "include/*.h")

# 根据选项构建目标
if(BUILD_EXECUTABLE)
    add_executable(${PROJECT_NAME} ${SOURCES})
    target_include_directories(${PROJECT_NAME} PUBLIC include)
    target_link_directories(${PROJECT_NAME} PUBLIC lib)
    # 如果需要链接第三方库
    # find_package(OpenGL REQUIRED)
    # target_link_libraries(${PROJECT_NAME} PRIVATE OpenGL::GL)
elseif(BUILD_STATIC_LIB)
    add_library(${PROJECT_NAME} STATIC ${SOURCES})
    target_include_directories(${PROJECT_NAME} PUBLIC include)
elseif(BUILD_SHARED_LIB)
    add_library(${PROJECT_NAME} SHARED ${SOURCES})
    target_include_directories(${PROJECT_NAME} PUBLIC include)
else()
    message(FATAL_ERROR "No build target selected. Please enable BUILD_EXECUTABLE, BUILD_STATIC_LIB, or BUILD_SHARED_LIB.")
endif()

# 处理调试/发布构建类型
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    message(STATUS "Building in Debug mode")
    # 设置调试特定的选项
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -DDEBUG")
elseif(CMAKE_BUILD_TYPE STREQUAL "Release")
    message(STATUS "Building in Release mode")
    # 设置发布特定的选项
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -DNDEBUG")
endif()

# 安装目标
if(BUILD_EXECUTABLE)
    install(TARGETS ${PROJECT_NAME} DESTINATION bin)
else()
    install(TARGETS ${PROJECT_NAME} DESTINATION lib)
    install(FILES ${HEADERS} DESTINATION include)
endif()

# 可选：自动化测试
# enable_testing()
# add_test(NAME TestExample COMMAND ${PROJECT_NAME} --test)

