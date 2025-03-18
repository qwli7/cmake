## CMake

一个管理和构建源代码的工具。



## 一个简单的 CMake 工程

创建一个目录，里面包含一个 `cpp` 文件和一个 `CMakeLists.txt`

```c++
#include <iostream>
int main() {
    std::cout << "Hello world" << std::endl;
    return 0;
}
```

```cmake
cmake_minimum_required(VERSION 3.10)
project(hello)
add_executable(hello main.cpp)
```

新建一个 `build` 目录，进入 `build` 目录执行配置命令；

```shell
cd build
cmake ..    # 选择上一级目录的 CMakeLists.txt 进行构建
```

`cmake` 默认支持多平台工程的生成，`-G` 参数可以指定生成对应 `IDE` 的工程，例如 `Visual Studio` 或者其他的。



### 1. cmake_minimum_required

```cmake
cmake_minimum_required(VERSION 3.10)
```

指定了运行当前 `CMakeLists.txt` 文件所需要的最小版本



### 2. project

```shell
project(hello)
```

 指定了当前项目的名称，全量语法如下

```cmake
project(<PROJECT-NAME> [<language-name>...])
project(<PROJECT-NAME>
        [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        [DESCRIPTION <project-description-string>]
        [HOMEPAGE_URL <url-string>]
        [LANGUAGES <language-name>...])
```

该命令会内置一些变量

```cmake
project(hello VERSION 1.21)
```

- 如果指定了项目名称，那么内置 `PROJECT_NAME` ，`PROJECT_SOURCE_DIR`，`PROJECT_BINARY_DIR`会生效
  - `PROJECT_SOURCE_DIR` 输出源代码目录；即顶层 `CMakeLists.txt` 所在的文件夹
  - `PROJECT_BINARY_DIR` 输出二进制文件存放目录；即 `build` 文件夹
- 如果指定了项目版本，那么内置 `PROJECT_VERSION` `PROJECT_VERSION_MAJOR` 等内置变量会生效



### 3. add_executable

```cmake
add_executable(main hello.cpp)
```

生成可执行文件命令，上面的命令表示生成一个名称为 `main` 的可执行文件，利用 `hello.cpp` 文件来生成

- 如果需要多个文件来生成，接着 `hello.cpp` 后面书写即可
- 可以用空格或者 `;` 来进行分隔

```cmake
add_executable(main hello.cpp;a.cpp;b.cpp)
add_executable(main hello.cpp a.cpp b.cpp)
```



##  指定目录配置

可以无需手动进入 `build` 目录执行配置，可以通过 `-B` 和 `-S` 来进行配置工作；

```shell
cmake -B 构建目录 -S 源码目录
#例如
cmake -B build/ -S .  # 指定当前的配置路径为当前目录下的 build 目录，指定顶层 CMakeLists.txt 文件的路径
```



## 构建可执行文件

配置完成后，一般会生成工程文件或者 `Makefile` 文件，假设生成了 `Makefile`  文件，可以手动执行 `make` 命令来生成可执行文件（不推荐），在项目中，我们一般通过 `cmake` 自带的 `build` 命令来生成

```shell
cmake --build build/  # 指定构建目录，并生成可执行文件
```



## 输出自定义调试信息 message

`cmake` 中提供了一个 `message` 指令，方便我们在配置和构建的过程中输出一些调试信息；

```cmake
message(STATUS "内容") # 输出标准内容，前面带有 --
message(SEND_ERROR "内容") # 输出错误内容，配置会继续，不会输出 Makefile文件
message(FATAL_ERROR "内容") # 输出严重错误，当前执行会被中断
```

通常我们使用 `STATUS` 即可，`STATUS` 为默认的级别，可以不写；



## 设置（set ）、获取（${}）变量

`cmake` 中提供 `set` 方法来供开发者定义变量，在 `cmake` 中，设置的变量都为字符串类型；

标准语法为：`set(变量名 变量值...)` ；

- 如果变量值有多个，可以用空格隔开
- 如果变量值中原本就带有空格，可以使用  `""`  将值包裹起来，让 `cmake` 把其当作一个整体

```cmake
set(MyVal hello) # 设置MyVal的值为 hello
set(MyVal2 "hello world") # 设置 MyVal2 的值为 hello world

message(${MyVal})
message(${MyVal2})
```



## 设置C++标准

在 `cmake` 中，我们可以通过 `set` 方法来设置当前配置和构建所使用的 `c++` 标准

```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True) #等同 set(CMAKE_CXX_STANDARD_REQUIRED ON)
```

- 设置了 `c++` 标准为 `11` ，

-  `CMAKE_CXX_STANDARD_REQUIRED` 设置 `c++` 标准是否为必须的，如果无效，则会回退到距离最近的一个 `c++` 版本进行编译



## 查找所有的源文件

之间在生成可执行文件时，我们利用了 `add_executable` 这个命令，该命令将生成目标所需的源文件全部拼接在后面；当我们工程项目中源文件过多时，手写添加显然不利于维护。`cmake` 提供了两种方式让我们在指定的目录中搜寻源代码文件

### aux_source_directory

完整语法如下 `aux_source_directory(搜寻目录 存储变量)`

```cmake
aux_source_directory(src SRC_LIST) 
message(${SRC_LIST})
```

执行上述命令后，将会把 `src` 目录下的所有源代码给列举出来，存储在 `SRC_LIST` 这个变量中；但该方法存在弊端

- 默认扫描源码文件类型为 `.c` 和 `.cpp`，不支持其他源码类型文件扫描
- 不支持递归查找，如果 `src` 目录下存在子目录中仍然有源文件，将无法被检索出来；

### file

`file` 解决了 `aux_source_directory` 中存在的一些问题，支持递归检索和扫描；`file` 的本质就是操作文件系统，除了查找源文件以外，还支持其他的高级操作；

查找文件的完整语法如下 `file({GLOB | GLOB_RECURSE} 输出变量 文件正则表达式...)`

```cmake
file(GLOB_RECURSE SRC_LIST2 src/*.c* src/**/*.c*)
message(${SRC_LIST2})
```

以上命令会输出 `src` 目录以及子目录下的所有 `.c` 和 `.cpp` 文件；如果需要扫描多个目录，在命令后面追加即可；



## configure_file

通常使用 `configure_file` 来处理文件；通常情况下，我们在 `CMakeFiles.txt` 文件中定义的变量是无法在 `.c/.cpp` 中访问的，`configure_file` 可以帮我们实现这种特殊的需求；

定义如下：`configure_file<输入文件 输出文件>`

- 输入文件路径是相对内置变量 `CMAKE_CURRENT_SOURCE_DIR` 来看的，并且必须是一个文件，而不能是一个文件夹
- 输出文件路径是相对内置变量 `CMAKE_CURRENT_BINARY_DIR`来看的

生成的过程中，会将输入文件中所有用  `@@` 以及 `${}` 包裹的变量进行替换

```cmake
# 定义一个输入的模板文件
#cmakedefine PROJECT_VERSION @PROJECT_VERSION@
#cmakedefine PROJECT_VERSION_MAJOR "@PROJECT_VERSION_MAJOR@"  #如果替换时想要被当成一个整体，那么应该在模板内使用"
#cmakedefine PROJECT_VERSION_MINOR ${PROJECT_VERSION_MINOR}
```

在 `CMakeFiles.txt` 中，利用 `configure_file` 去生成文件

```cmake
# template 和 CMakeLists在同级下，可以省略 ${CMAKE_CURRENT_SOURCE_DIR}
# 输出文件默认在 ${CMAKE_CURRENT_BINARY_DIR}下，也可以指定其他输出文件
configure_file(template config_head.h) 
```

在 `CMake` 配置执行完成后，变量将会被替换，这样我们就可以在源代码中使用预编译指令 `#include` 包含该头文件，对定义的变量进行使用了；

```cpp
//cmake 配置后，输出的 config_head.h 里面的内容
#define PROJECT_VERSION 1.11
#define PROJECT_VERSION_MAJOR "1"
#define PROJECT_VERSION_MINOR 11
```



## option

`option` 通常用来给用户提供一个可选的布尔值，取值只能是 `True/False` 或者 `ON/OFF`；可以和 `configure_file`  来搭配使用

定义如下 `option(属性名 <属性描述> 属性值)`

```cmake
option(DEBUG_ENABLE "对DEBUG_ENABLE的描述" ON) # 设置当前值为开启状态
```

和 `if` 以及 `configure_file` 搭配使用

在 `template` 模板文件中，添加 `DEBUG_STRING` 定义

```cmake
#cmakedefine DEBUG_STRING "${DEBUG_STRING}"
```

在 `CMakeLists.txt` 中，利用 `option` 定义一个可选值，当值为 `True` 或者 `ON` 时，才设置变量 `DEBUG_STRING`

```cmake
option(ENABLE_DEBUG "是否启用 debug" ON)
if(ENABLE_DEBUG)
    set(DEBUG_STRING "this is a debug string")
endif()

configure_file(template config_head.h)
```

- 值为 `ON`，生成的 `config_head.h` 头文件中，将会定义 `DEBUG_STRING` 这个宏
- 值为 `OFF`，生成的 `config_head.h` 头文件中，`DEBUG_STRING` 这个宏将会被注释掉；例如 `/* #undef DEBUG_STRING */`

以上搭配，可以用来在源代码中进行一些流程控制；



## cmakedefine 和 define 的区别

- `cmakedefine` 如果变量没有被定义，生成配置文件时，宏将会被注释；例如 `/* #undef DEBUG_STRING */`
- `define` 如果变量没有被定义，生成配置文件时，宏将会被设置成空

```cmake
#------- template-------------
#cmakedefine DEBUG_STRING "${DEBUG_STRING}"
#define DEBUG_STRING2 "${DEBUG_STRING}"

#------- CMakeLists.txt-------------
option(ENABLE_DEBUG "是否启用 debug" OFF)
if(ENABLE_DEBUG)
    set(DEBUG_STRING "this is a debug string")
endif()
configure_file(template config_head.h)

#------- config_head.h-------------
/* #undef DEBUG_STRING */      -> cmakedefine
#define DEBUG_STRING2 ""       -> define
```



## include_directories（不推荐使用）

使用 `configure_file` 生成了头文件之后，我们就可以在代码中引入头文件，并且使用头文件中定义的变量了；但当我们使用 `cmake --build target/` 去构建项目时，会得到一个错误，提示无法找到 `config_head.h`，这是因为我们没有给程序指明头文件所在的路径；`include_directories` 就是用来做这件事的；

语法定义如下`include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])`

在 `CMakeLists.txt` 中，我们可以通过它去指明我们的头文件包含目录

```cmake
#configure_file 默认生成的文件就在 CMAKE_CURRENT_BINARY_DIR 目录下
include_directories(${CMAKE_CURRENT_BINARY_DIR})  
```

再执行构建就可以正常打印我们在源码中引用的变量了；



## target_include_directories（推荐）

和 `include_directories` 的作用一样，只是加了一些限制；`include_directories` 默认对所有的目标对象都添加该包含路径，`target_include_directories` 在其的基础之上，添加了目标属性和传播属性，指定该依赖只针对特定的对象添加

语法定义如下 

```cmake
target_include_directories(<target> [SYSTEM] [AFTER|BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
```

在 `CMakeLists.txt` 中，我们将 `include_directories` 替换成 `target_include_directories`

```cmake
target_include_directories(hello PRIVATE ${CMAKE_CURRENT_BINARY_DIR})
```



## add_library

```shell
.
├── CMakeLists.txt
├── calc.cpp
└── calc.h
```

`add_library` 将一些指定的文件添加到一个库中；

语法定义如下：`add_library(库名称 [STATIC|SHARED|MODULE] [源代码路径])`

库的类型分为三种，这里只讨论前两种

- `STATIC` 静态库，也是默认的类型；`add_library(Math ${MATH_SCRS})` 或者 `add_library(Math STATIC ${MATH_SCRS})`
- `SHARED` 共享库，也称动态库；`add_library(Math SHARED ${MATH_SCRS})`

```cmake
# 将当前 CMakeLists.txt 下面所有的源文件打包到 MATH_SCRS 变量中
file(GLOB MATH_SCRS ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)
# 将所有的文件打包成一个静态库
add_library(Math ${MATH_SCRS}) 
# 是否需要将库中的头文件添加到包含目录中
# 确认库中的头文件在外部是否被使用，如果被使用，那么就应该将其添加到包含目录中，反之则不需要
target_include_directories(Math INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
```



在顶层的 `CMakeLists.txt` 中，如果需要链接 `Math` 这个库，需要做如下改动

```cmake
# top CMakeLists.txt
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/src/math) #将子目录包含进来
target_link_libraries(hello Math) #给目标对象去链接 Math 数据库
```



## PUBLIC、PRIVATE、INTERFACE 的区别

以下是通俗的解释

- `PUBLIC` 当前目标需要这个路径，其他目标也需要这个路径
- `PRIVATE` 当前目标需要这个路径，其他目标不需要这个路径
- `INTERFACE` 当前目标不需要这个路径，其他目标需要这个路径

所以在上面的 `Math` 库中，因为头文件和源码文件在同一个目录下。本目标不需要这个路径，而其他目标需要这个路径，所以可填 `INTERFACE` 或者 `PUBLIC`



## link_libraries（不推荐使用）

`link_libraries` 为所有目标添加指定的链接库；在连接之前，需要手动调用 `include_directories` 将库的头文件目录包含进来；

语法定义如下：`link_libraries([items]...)`

针对 `Math` 库，在顶层的 `CMakeLists.txt` 中可以这样编写

```cmake
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/math)
link_libraries(Math)
```



## target_link_libraries（推荐）

`target_link_libraries` 给指定的目标连接指定的库，可以更加细粒度固定传播范围；推荐使用该方式

语法定义如下：`target_link_libraries(<target> ... <item>... ...)`

同样也可以设定传播范围

针对 `Math` 的例子，在子 `CMakeLists.txt`  中可以做如下修改

```cmake
file(GLOB MATH_SRCS  ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)
add_library(Math STATIC ${MATH_SRCS})
target_include_directories(Math INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
```

在顶层 `CMakeLists.txt` 中

```cmake
#给目标对象链接对应的库即可
target_link_libraries(hello Math)
```



## install

`install` 命令用于定义项目的安装规则，通过 `make install` 或者 `cmake --install` 执行时，会将目标文件、库、头文件等复制都指定目录；

基本语法定义

```cmake
install(TARGETS <target1> <target2>...
    [ARCHIVE|LIBRARY|RUNTIME|FRAMEWORK|BUNDLE|...]  # 指定目标类型
    DESTINATION <路径>                               # 安装路径（相对或绝对）
    [PERMISSIONS <权限>]                             # 文件权限（如 OWNER_READ）
    [COMPONENT <组件名>]                             # 分组安装（可选）
)
```

