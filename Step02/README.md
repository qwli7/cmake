## 步骤二: 添加库
我们已经知道如何使用 `CMake` 创建一个简单的工程；在这一步中，我们将会学会在我们的项目中使用一个库；也会学习如何将我们的库变成一个可选的动作

### 练习1 - 创建一个库
要在 `CMake` 中添加一个库，需要使用 `add_library()` 去指明，并指定哪些源文件应该组成该库；在我们的项目中，我们会使用一个或者多个目录去组织我们的项目，而不是将所有的源代码都放在一个目录中；在这个案例中，我们将为我们的库创建一个单独的子目录。在这里，我们将添加一个新的 `CMakeLists.txt` 文件和一个或者多个源文件；在顶层的 `CMakeLists.txt` 文件中，我们将使用 `add_subdirectory()` 命令将我们的子文件夹加入到构建中。

一旦这个库被创建，它将被 `target_include_directories()` 和 `target_link_libraries()` 链接到我们最终的可执行文件中。

#### 1.目标
添加并且使用一个库

#### 2.有用的资源
- `add_library()`   指定哪些文件被包含在库中
- `add_subdirectory()`  添加指定的文件，将指定的文件加入到构建中
- `target_include_directories()` 链接库的头文件
- `target_link_libraries()` 链接库 
- `PROJECT_SOURCE_DIR`  项目源文件目录


#### 3.要修改的文件
- `CMakeLists.txt`
- `tutorial.cxx`
- `MathFunctions/CMakeLists.txt`

#### 4.开始（`Getting Started`）
在这个练习里面，我们会在我们的项目中添加一个库，这个库包含了我们实现计算平方根的逻辑；最终可执行文件中我们使用这个库来替代标准库来进行计算

在这个教程中，我们会将库放置到一个叫做 `MathFunctions` 的子文件夹中，这个目录已经包含了 `MathFunctions.h` 和 `mysqrt.h` 这两个头文件。他们的源代码 `MathFunctions.cxx` 和 `mysqrt.cxx` 也已被提供。我们不需要修改这些文件中的任何一个。

在 `Step02` 文件夹中，开始完成 `TODO 1` 和 `TODO 6`；

首先，在 `MathFunctions` 子目录中的 `CMakeLists.txt` 中填上一行

然后，修改顶层的 `CMakeLists.txt`

最后，在 `tutorial.cxx` 中使用新创建的 `MathFunctions` 库


#### 5.构建和运行（`Build and Run`）
运行 `cmake` 可执行程序或者 `cmake-gui` 工具来配置项目，然后选择你使用的构建工具来构建它；

你执行的命令行应该像这样
```cmd
mkdir Step2_build
cd Step2_build
cmake ../Step2
cmake --build .
```

尝试去使用新的 `Tutorial` 并确保它仍然能正确计算平方根

#### 6.解决方案（`Solution`） 
在 `MathFunctions` 目录中的 `CMakeLists.txt` 中，我们通过 `add_library()` 方法来创建一个名为 `MathFunctions` 的库，库的源文件作为参数传递给了 `add_library()`；
```cmake
#TODO1
add_library(MathFunctions MathFunctions.cxx mysqrt.cxx)
```

为了使用新库，我们在顶层 `CMakeLists.txt` 中通过 `add_subdirectory()` 调用，来构建新库
```cmake
#TODO2
add_subdirectory(MathFunctions)
```

然后，新库应该被 `target_link_libraries()` 链接到可执行程序中
```cmake
#TODO3
target_link_libraries(Tutorial PUBLIC MathFunctions)
```

最后，我们需要指明库的头文件位置；修改现有的 `target_include_directories()` 调用，将 `MathFunctions` 子目录也添加到里面，方便 `MathFunctions.h` 头文件能被找到；
```cmake
#TODO4
target_include_directories(Tutorial PUBLIC "${PROJECT_BINARY_DIR}" "${PROJECT_SOURCE_DIR}/MathFunctions")
```

现在我们开始使用我们的库，在 `tutorial.cxx` 中包含 `MathFunctions.h`
```cpp
#TODO5
#include "MathFunctions.h"
```

最后，使用我们封装好的 `mathfunctions::sqrt` 替换 `sqrt` 
```cpp
const double outputValue = mathfunctions::sqrt(inputValue)
```



### 练习二 - 添加一个选项
让我们在 `MathFunctions` 库中添加一个选项，允许开发者去选择使用我们自己封装的库或者使用标准实现；对于教程来说，确实是没有必要这样实现，但对于大型项目来说，这是比较常见的；

`CMake` 的 `option()` 命令能做到这一点，这位用户提供了一个在 `cmake` 构建时可以更改的变量；此设置将存储在缓存中，这样用户就无需在每次在构建目录上运行 `CMake` 时设置该值

#### 1.目标
添加不使用 `MathFunctions` 的选项

#### 2.有用的资源
- `if()`  条件判断语句
- `option()` 选项
- `target_compile_definitions()`  


#### 3.要修改的文件
- `MathFunctions/CMakeLists.txt`
- `MathFunctions/MathFunctions.cxx`

#### 4.开始（`Getting Started`）
以练习一的结果作为开始，完成 `TODO 7` 到 `TODO 14`

首先，在 `MathFunctions/CMakeLists.txt` 文件中使用 `option()` 创建一个 `USE_MYMATH` 变量，使用该选项将编译定义传递给 `MathFunctions` 库

然后，更新 `MathFunctions.cxx`，基于 `USE_MYMATH` 的值进行重新编译

最后，通过将 `mysqrt.cxx` 设为 `MathFunctions/CMakeLists.txt` 的 `USE_MYMATH` 块内的自己的库，防止在 `USE_MYMATH` 开启时对其进行编译

#### 5.构建和运行（`Build and Run`）
我们在练习一中已经配置过我们的构建目录了，可以通过以下的命令调用来重新构建
```cmd
cd ../Step2_build
cmake --build .
```

然后运行 `Tutorial` 可执行程序，通过几个数字来验证它是否仍然正确

现在我们更新 `USE_MYMATH` 的值为 `OFF`，最简单的方式仍然是使用 `cmake-gui`，如果你仍然在终端中，可以使用`cmake`；如果你想通过命令行来改变选项的值，可以使用如下命令
```cmake
cmake ../Step2 -DUSE_MYMATH=OFF
```

使用如下的命令来重新构建
```cmake
cmake --build .
```
然后，再一次运行可执行文件，确保它在 `USE_MYMATH` 设置为 `OFF` 的时候，仍然可以工作

#### 6.解决方案（`Solution`） 
在 `MathFunctions/CMakeLists.txt`中添加 `USE_MYMATH`选项
```cmake
#TODO7
option(USE_MYMATH, "Use tutorial provided math implementation" ON)
```

接下来，构建并且链接我们的库，使 `mysqrt` 这个函数成为有条件的。

创建一个 `if()` 语句，用来检查 `USE_MYMATH` 的值。在 `if()` 代码块的内部，添加 `target_compile_definitions()` 命令来编译定义 `USE_MYMATH`；
```cmake
#TODO8
if(USE_MYMATH) 
    target_compile_definitions(MathFunctions PRIVATE "USE_MYMATH")
endif()
```

当 `USE_MYMATH` 值为 `ON`，编译定义 `USE_MYMATH` 将会被设置；我们可以在我们的源代码中使用这个编译定义去启用/禁用代码片段

源代码的更改也比较简单，在 `MathFunctions.cxx`，我们通过 `USE_MYMATH`来控制我们使用哪个平方根方法

```cpp
//TODO9
#ifdef USE_MYMATH
return detail::mysqrt(x);
#else
return std::sqrt(x);
#endif
```

接下来，我们需要引入头文件，如果 `USE_MYMATH` 定义了
```cpp
//TODO10
#ifdef USE_MYMATH
#include "mysqrt.h"
#endif
```

最后，我们需要引入标准的 `cmath` 库，当我们使用标准 `std::sqrt()` 时需要使用它
```cpp
//TODO11
#include <cmath>
```
如果 `USE_MYMATH` 被设置成 `OFF`， `mysqrt.cxx` 不会被使用，但它仍然会被编译，因为 `MathFunctions` 目标列表里面有 `mysqrt.cxx`；有几种方式来修正这个问题。第一个方法就是使用 `target_source()` 去添加 `mysqrt.cxx`，另一个方法时就是通过 `USE_MYMATH` 创建可以额外的库，在这个教程中，我们将会使用第二种方式；

首先，从 `USE_MYMATH` 中创建一个名为 `SqrtLibrary` 的库，它的源代码就是 `mysqrt.cxx`

```cmake
#TODO12
add_library(SqrtLibrary PUBLIC mysqrt.cxx)
```

接下来，我们链接 `SqrtLibrary` 到 `MathFunctions` 中，当 `USE_MYMATH` 是启用的时候；

```cmake
#TODO13
target_link_libraries(MathFunctions PRIVATE SqrtLibrary)
```

最后我们移除 `mysqrt.cxx` 从 `MathFunctions` 库中，因为他已经被 `SqrtLibrary` 包含了
```cmake
add_library(MathFunctions MathFunction.cxx)
```

上述改变做完后，`mysqrt` 方法已经变成一个可选的，使用者可以通过 `USE_MYMATH` 的值进行切换来控制是否编译它，使用它 

