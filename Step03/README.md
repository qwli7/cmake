## 步骤三：添加库的使用依赖
### 对依赖字段的解释
```cmake
PUBLIC: 本目标需要用，依赖这个目标的其他目标也需要用
INTERFACE: 本目标不需要，依赖本目标的其他目标需要
PRIVATE: 本目标需要，依赖这个目标的其他目标不需要

通俗来讲，就是你提供了一个库，你自己的 CMakeLists.txt 文件就直接把导入头文件的动作直接做了，别人使用时，只需要链接你的库就行，而不需要再去导入头文件

所以在 MathFunctions 的 CMakeLists.txt 中添加了导入代码；
target_include_directories(MathFunctions INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}")
这里使用 INTERFACE 是引文头文件和源码本身就在同一个目录内，所以本目标不需要，依赖本目标的其他目标需要，所以改成了INTERFACE
```

### 练习1 - 为一个库添加使用依赖
目标参数的使用要求允许更好地控制库或可执行文件的链接和包含行，同时还可以更好地控制 `CMake` 内部目标的传递属性。利用使用要求的主要命令是：
- `target_compile_definitions()`
- `target_compile_options()`
- `target_include_directories()`
- `target_link_directories()`
- `target_link_options()`
- `target_precompile_headers()`
- `target_sources()`

以上几个命令的参数中都有 `PUBLIC`、`INTERFACE` 和 `PRIVATE` 属性用来控制传递；

#### 1.目标
为一个库添加使用要求

#### 2.有用的资源
- `CMAKE_CURRENT_SOURCE_DIR`  当前源代码目录


#### 3.要修改的文件
- `MathFunctions/CMakeLists.txt`
- `CMakeLists.txt`

#### 4.开始（`Getting Started`）
在这个练习中，我们会使用现代 `CMake` 模式来重构 `Step2 添加库` 中代码；让我们的库定义自己的使用要求，以便在必要时，将他们传递给其他目标；在这个案例中 `MathFunctions` 将指定任何所需包含目录本身，然后消费目标只需要链接到 `MathFunctions`，而不需要担心需要添加额外的包含目录；


开始的源代码已在 `Step3` 目录中提供；完成 `TODO 1` 到 `TODO 3`

首先，在 `MathFunctions/CMakeLists.txt` 添加一个 `target_include_directories()` 调用；记住 `CMAKE_CURRENT_SOURCE_DIR` 是当前正在处理源代码的路径；

然后更新顶层 `CMakeLists.txt` 中的 `target_include_directories()` 调用

#### 5.构建和运行（`Build and Run`）
创建一个 `Step3_build` 的目录，运行 `cmake` 可执行程序或者 `cmake-gui` 工具来配置项目，然后选择你使用的构建工具来构建它；

你执行的命令行应该像这样
```cmd
mkdir Step3_build
cd Step3_build
cmake ../Step3
cmake --build .
```

接下来验证新生成的 `Tutorial` 是否如预期一样工作；


#### 6.解决方案（`Solution`） 
我们知道，任何链接到 `MathFunctions` 的目标都需要包含源目录，只有 `MathFunctions` 自己本身不需要；所以它可以被表达成 `INTERFACE`（个人理解：因为他们本身就在同一个目录）；记住：`INTERFACE` 意味着消费者需要，而提供者不需要；

在 `MathFunctions/CMakeLists.txt` 的结尾，使用 `target_include_directories()`，搭配 `INTERFACE` 使用；

```cmake
#TODO1
target_include_directories(MathFunctions INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}")
```

现在我们为 `MathFunctions`规范了使用依赖，我们可以从顶层 `CMakeLists.txt` 中安全的移除 `EXTRA_INCLUDES` 变量；

```cmake
#TODO2 移除这一行，就是不需要把 MathFunctions 的头文件添加到 include 目录了，因为库自己本身做了 include 动作
list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")

```

然后再移除 `EXTRA_INCLUDES` 从 `target_include_directories()`
```cmake
#TODO3 将TODO3下面的 target_include_directories 换成下面这行；
target_include_directories(Tutorial PUBLIC "${PROJECT_BINARY_DIR}")
```
**请注意**，使用此技术，我们的可执行目标使用库的唯一操作是使用库目标的名称调用 `target_link_libraries()`，省略了导入头文件的动作；但在大型项目中，这种手动指定库依赖项的方法很快就会变得非常复杂；（个人理解：因为依赖的库变多，手动指定会十分麻烦）


### 练习2 - 使用接口库设定 C++ 标准
现在我们已经将代码转换成更加现代的 `CMake` 写法，让我们演示一种将属性设置为多个目标的方法；

我们将使用接口库（`INTERFACE Library`）重构我们的代码，我们将在 `Step04` 中使用该库来演示生成器表达式的常见用途；

#### 1.目标
添加一个 `INTERFACE Library` 来指定所需要的 `C++` 标准 

#### 2.有用的资源
- `add_library()`  
- `target_compile_features()` 
- `target_link_libraries()`  


#### 3.要修改的文件
- `CMakeLists.txt`
- `MathFunctions/CMakeLists.txt`

#### 4.开始（`Getting Started`）
在这个练习中，我们会使用 `INTERFACE library` 重构我们的代码来指定 `C++` 标准；本例中你需要完成 `TODO4` 到 `TODO 7`

从顶层的 `CMakeLists.txt` 开始，使用 `tutorial_compiler_flags` 来构建一个 `INTERFACE library`，并且指定 `cxx_std_11` 作为目标编译特征

修改 `CMakeLists.txt` 和 `MathFunctions/CMakeLists.txt` 以便所有的目标都有一个对 `tutorial_compiler_flags` 的 `target_link_libraries()` 调用。


#### 5.构建和运行（`Build and Run`）
我们已经创建过构建目录了，所以只需要执行以下命令进行重新构建
```cmd
cd Step3_build
cmake --build .
```
验证 `Tutorial` 是否可以如预期进行工作


#### 6.解决方案（`Solution`） 
首先，移除两行设置 `C++` 标准的代码，他们是用 `set()` 设置的；
```cmake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```

接下来，我们需要创建一个 `interface library`， `tutorial_compiler_flags`，并且在 `target_compile_features()` 中去添加编译特征 `cxx_std_11`

```cmake
#TODO4
add_library(tutorial_compiler_flags INTERFACE)
target_compile_features(tutorial_compiler_flags INTERFACE cxx_std_11)
```
最后，设置好接口库后，我们需要将可执行 `Tutorial`，`SqrtLibrary` 库和 `MathFunctions` 库链接到新的 `tutorial_compiler_flags` 库。代码如下：
```cmake
#TODO5
target_link_libraries(Tutorial PUBLIC MathFunctions tutorial_compiler_flags)

# TODO6
target_link_libraries(SqrtLibrary PUBLIC tutorial_compiler_flags)

# TODO7
target_link_libraries(MathFunctions PUBLIC tutorial_compiler_flags)
```


