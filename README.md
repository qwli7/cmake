## `CMake` 官方学习文档

翻译采用个人习惯进行翻译

- [Step01 一个基本的入口点](Step01/README.md)
- [Step02 添加库](Step02/README.md)


## `CMake` 常用指令
- `cmake_minimum_required(VERSION <version>)` 指定运行此 `CMakeLists.txt` 文件的最低 `CMake` 版本
- `project(<project_name> [<language> ...])` 设定项目名称和语言，也可设置版本号等，运行时会设置 `CMAKE_PROJECT_NAME` 属性
    - 执行后，内置四种变量
        - `<PROJECT-NAME>_VERSION_MAJOR` 主版本
        - `<PROJECT-NAME>_VERSION_MINOR` 次版本
        - `<PROJECT-NAME>_VERSION_PATCH`
        - `<PROJECT-NAME>_VERSION_TWEAK`  
    ```cmake
    # project(<project_name> [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]])
    # 执行后，内置四种变量
    project(cmake_demo VERSION 3.1.2.1)

    message(STATUS ${${CMAKE_PROJECT_NAME}_VERSION_MAJOR}) #3
    message(STATUS ${${CMAKE_PROJECT_NAME}_VERSION_MINOR}) #1
    message(STATUS ${${CMAKE_PROJECT_NAME}_VERSION_PATCH}) #2
    message(STATUS ${${CMAKE_PROJECT_NAME}_VERSION_TWEAK}) #1
    ```

- `add_executable(<target> <source_files>...)` 指定要生成的可执行文件和源文件
    ```cmake
        add_executable(MyExecutable main.cpp other_file.cpp)
    ```
- `add_library(<target> <source_files>...)` 创建一个库文件（静态库或者动态库，默认是静态库）
    ```cmake
    add_library(MyLibrary STATIC library.cpp) #静态库，不指定 STATIC，也默认为 STATIC
    add_library(MyLibrary SHARED library.cpp) #共享库，动态库

    #可以定义 BUILD_SHARED_LIBS 这个属性，告诉 cmake 当这个属性开启时，不指定会默认构建成动态库
    option(BUILD_SHARED_LIBS "Building using shared libraries" ON) 

    ```
- `target_link_libraries(<target> <libraries>...)`  链接目标文件和其他库
    ```cmake
    target_link_libraries(MyExecutable MyLibrary) 
    ```
- `include_directories(<dirs>...)` 添加头文件搜索路径
    - 使用该属性后，目录会被填充到 `INCLUDE_DIRECTORIES` 中，生成器将使用该属性为编译器设置包含目录（头文件目录）
    - 推荐使用 `target_include_directories()` 替代，可以设置依赖关系
    ```cmake
    include_directories(${PROJECT_SOURCE_DIR}/include)
    ```
- `target_include_directories(<target> <INTERFACE|PUBLIC|PRIVATE> [items1...] <INTERFACE|PUBLIC|PRIVATE> [items2...]...)` 给目标添加包含目录
    - 编译给定的目标时，指定包含目录；多次调用会被追加
    - `target` 必须是被 `add_executable()` 或者 `add_library()` 命令创建的对象

- `set(<variable> <value>...)` 设置变量的值

- `option(<variable> "<help_text>" [value])` 提供一个选项值，OFF是默认值，该值一旦被设置后，将会被缓存；

- `configure_file(<input> <output>)` 从 input 拷贝文件到 output，
    - input 路径是从 `CMAKE_CURRENT_SOURCE_DIR` 开始
    - output 输出路径默认是从 `CMAKE_CURRENT_BINARY_DIR` 处理，可以指定输出路径
    ```cmake
    configure_file(common.h.in, common.h); //common.h 放置到可执行文件同级目录
    configure_file(common.h.in, "include/common.h"); //可执行文件创建文件夹
    ```

- `message([<mode>] "message text...")` 打印日志，`mode` 通常设置为 `STATUS` 即可，常见的可以参考 ![message mode 定义](https://cmake.org/cmake/help/latest/command/message.html)
    ```cmake
    message(STATUS "hello world"); //打印消息
    ```
- `aux_source_directory(<dir> <variable>)` 查找源文件，指定的目录中  
    ```cmake
    aux_source_directory(. SRC_LIST) # 从当前的目录中，查找所有的源文件，放到 SRC_LIST 中
    message(STATUS ${SRC_LIST})
    ```