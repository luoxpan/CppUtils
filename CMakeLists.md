

## 基础版本：自行指定一个或多个.cpp创建可执行文件
```CMake
cmake_minimum_required(VERSION 3.15)
project(STL)

# 设置 C++ 标准为 C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON) #C++17是必须的

# 设置可执行文件输出的目录
set(HOME ${CMAKE_CURRENT_SOURCE_DIR})
set(EXECUTABLE_OUTPUT_PATH ${HOME}/bin)
set(SRC ${HOME}/src)
set(TEST ${HOME}/test)
# 添加头文件目录
include_directories(${PROJECT_SOURCE_DIR}/include)

# 创建可执行文件，名称为 test
add_executable(memory_test ${TEST}/memory_test.cpp)
```

## 版本2：src文件夹所有文件创建一个可执行文件
```CMake
cmake_minimum_required(VERSION 3.15)
project(STL)

# 设置 C++ 标准为 C++17
set(CMAKE_CXX_STANDARD 17)

# 设置可执行文件输出的目录
set(HOME ${CMAKE_CURRENT_SOURCE_DIR})
set(EXECUTABLE_OUTPUT_PATH ${HOME}/bin)
set(SRC ${HOME}/src)
set(TEST ${HOME}/test)
# 添加头文件目录
include_directories(${PROJECT_SOURCE_DIR}/include)

# 搜索 src 目录下的源文件
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC_LIST)

# 创建可执行文件，名称为 main，源文件由 SRC_LIST 变量提供
add_executable(main ${SRC_LIST})
```

## 版本3：文件夹下的每个.cpp单独创建一个可执行文件
```CMake
cmake_minimum_required(VERSION 3.15)    #cmake 最低版本
project(PrimerPlus) 
set(CMAKE_CXX_STANDARD 17)  #使用C++17
set(HOME ${CMAKE_CURRENT_SOURCE_DIR})  #项目目录
set(EXECUTABLE_OUTPUT_PATH ${HOME}/bin) #生成文件目录
set(SRC ${HOME}/src)    #源码目录
#添加头文件目录
include_directories(${PROJECT_SOURCE_DIR}/include)  #头文件目录

# 编译第几章
set(COMPILE_CHAPATER 2)

file(GLOB_RECURSE CPP_LIST ${SRC}/${COMPILE_CHAPATER}/*.cpp) #找出src/2中所有cpp文件
foreach(v ${CPP_LIST})
    string(REGEX MATCH "2/.*" relative_path ${v})   #匹配查找2/*,查找结果为2/2_3.cpp
    string(REGEX REPLACE "2/" "" target_name ${relative_path})  #将"2/"替换为空，替换结果为2_3.cpp
    string(REGEX REPLACE ".cpp" "" target_name ${target_name})  #去除后缀,结果为2_3
    add_executable(${target_name} ${v})
endforeach()
```

## 版本4：VSCODE+Cmake+QT6 
```CMake
cmake_minimum_required(VERSION 3.5.0)
project(chess VERSION 0.1.0 LANGUAGES C CXX)

set(TARGET_NAME chess)
# QT相关
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

set(CMAKE_CXX_STANDARD 17)  #C++标准为17
set(CMAKE_CXX_STANDARD_REQUIRED ON) #C++17是必须的

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)  #生成compile_commands.json，包含了所有编译命令

set(QT6_PATH "D:\\Program\\Qt6\\6.7.2\\mingw_64\\lib\\cmake")
set(CMAKE_PREFIX_PATH ${QT6_PATH})  
find_package(Qt6 REQUIRED COMPONENTS Widgets)
qt_standard_project_setup()

###自定义
set(HOME_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(EXECUTABLE_OUTPUT_PATH ${HOME_DIR}/bin)
include_directories(${HOME_DIR}/include)

aux_source_directory(${HOME_DIR} MAIN_SOURCE)
aux_source_directory(${HOME_DIR}/src APP_SOURCE)
add_executable(${TARGET_NAME} ${MAIN_SOURCE} ${APP_SOURCE})
target_link_libraries(${TARGET_NAME} PRIVATE Qt6::Widgets)

set_target_properties(${TARGET_NAME} PROPERTIES
    WIN32_EXECUTABLE OFF
    MACOSX_BUNDLE ON
)

```
