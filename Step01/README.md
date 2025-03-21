## 步骤一: 一个基本的入口点程序
我从哪里开始使用 `CMake`？这一步将介绍一些 `CMake` 的基本语法、命令和变量。随着这些概念的介绍，我们将通过以下三个练习并创建一个简单的 `CMake`项目。

在这个这段中，每个练习的开始都会有一些背景信息。然后提供一个目标和一系列有用的资源文件；在 `Step1` 目录中每个文件的 `File to Edit` 部分都包含一些 `TODO` 的注释；每一个 `TODO` 注释都表示当前行需要去新增或者修改一到两行代码；这些 `TODO` 需要按照顺序去完成，先完成 `TODO1`，然后是 `TODO2` 依此类推；开始部分 `Getting Started` 会在你完成练习之前，给你一些有用的提示和指引；然后 `Build and Run` 部分将会一步一步的告诉你如何去构建和测试当前练习；最后，在每个练习结束的时候会讨论当前练习的解决方案；

注意：每一步的练习都建立在上一步的基础上。例如，`Step02` 的开始代码就是 `Step01` 的完成代码；

### 练习1 - 构建一个基本的工程
最基本的 `CMake` 工程是从一个单独的源文件构建得来的一个可执行目标；对于这样简单的项目，只需要一个包含三行代码的 `CMakeLists.txt` 文件

**注意：** 尽管 `CMake` 支持大写、小写以及混合写法的命令，但小写命令仍然是首选并且在整个教程中会被使用；

任何工程的 `CMakeLists.txt` 的顶部都必须使用 `cmake_minimum_required()` 命令来指定 `CMake` 使用的最低版本作为开始；这将建立一个策略的设定：确保兼容以下的 `CMake` 函数和 `CMake` 版本

为了开始项目，我们使用 `project()` 命令去设置项目的名称。当前调用是每个项目都必须的，且要求在 `cmake_minimum_required()` 之后调用；我们待会会看到，这个命令还能被指定其他项目级别的信息，例如语言或者版本信息；

最后，`add_executable()` 命令会告诉 `CMake` 使用指定的源文件去创建一个可执行的程序


#### 1.目标
理解如何去创建一个简单的 `CMake` 工程

#### 2.有用的资源
- `add_executable()`  //创建一个可执行的资源，根据给定的源文件
- `cmake_minimum_required()` //指定 CMake 的最小版本号
- `project()`  //指定项目名称或者其他项目级别的信息


#### 3.要修改的文件
- `CMakeLists.txt`

#### 4.开始（`Getting Started`）
在 `Step01` 目录中提供了一个 `tutorial.cxx`的源文件，用来计算给定数字的平方根；这个文件在当前步骤中不需要被更新；

同样，在 `Step01` 目录中，你需要完成的文件是 `CMakeLists.txt`, 开始完成 `TODO1` 和 `TODO3` 吧


#### 5.构建和运行（`Build and Run`）
一旦 `TODO1` 和 `TODO3` 被完成，我们就可以去构建和运行我们的项目。首先运行 `cmake` 命令或者 `cmake-gui` 工具去配置项目，然后使用你选择的构建工具去构建它；

例如：我们可以通过命令行工具导航到 `Step01` 的上一级目录，然后创建一个构建目录；
```cmd
mkdir Step1_build
```
然后导航到构建目录，并且运行 `cmake` 命令去配置项目并且生成本地构建系统
```cmd
cd Step1_build
cmake ../Step1
```
然后调用构建系统去真实编译/链接项目
```cmd
cmake --build .
```
对多配置生成器（例如 `Visual Studio`），首先导航到合适的子目录；
```cmd
cd Debug
```
最后试一下最新构建的可执行程序 `Tutorial`
```cmd
Tutorial 4294967296
Tutorial 10
Tutorial
```

**注意**：依赖不同的 `shell`，正确的语法可能是 `Tutorial`，`./Tutorial`，或者 `.\Tutorial`，在本练习中使用第一个

#### 6.解决方案（`Solution`） 
略，详细可查看 [CMakeList.txt](CMakeLists.txt) 中的 `TODO1` 和 `TODO3`

```cmake
cmake_minimum_required(VERSION 3.10)
project(Tutorial) 
add_executable(Tutorial Tutorial.cxx)
```


### 练习2 - 指定 `C++` 的标准
`CMake` 有一些特殊的变量，这些变量要么是在幕后被创建的，要么是在项目中被设置的；这些大多数的变量都是以 `CMake_` 开头，在你的项目中，应当尽量避免和 `CMake` 的内置变量命名冲突；其中有两个变量用户可在构建项目时需要设置的，它们是 `CMAKE_CXX_STANDARD` 和 `CMAKE_CXX_STANDARD_REQUIRED`

#### 1.目标
添加一个需要 `C++11` 支持的新特性

#### 2.有用的资源
- `CMAKE_CXX_STANDARD`
- `CMAKE_CXX_STANDARD_REQUIRED`
- `set()`

#### 3.需要修改的文件 （`Files to Edit`）
- `CMakeLists.txt`
- `tutorial.cxx`

#### 4.开始（`Getting Started`）
继续编辑 `Step01` 目录中的文件，完成步骤 `TODO 4` 和 `TODO 6`

首先，编辑 `tutorial.cxx` 添加一个 `C++11` 的新特性，然后更新 `CMakeLists.txt` 文件，去设置需要 `C++11` 的标准；

#### 5.构建和运行（`Build and Run`）
让我们来再一次构建我们的项目，之前我们已经创建过一个构建目录并且已经运行过 `Exercise 1`，我们可以跳过直接执行构建步骤；
```cmd
cd Step1_build
cmake --build .
```

现在我们可以我们新构建的 `Tutorial`，用法和之前使用的一样
```cmd
Tutorial 4294967296
Tutorial 10
Tutorial
```

#### 6.解决方案（`Solution`）
略过，具体可以查看 [tutorial](tutorial.cxx) 中的 `TODO 4` 和 `TODO 5` 以及 [CMakeLists z](CMakeLists.txt) 中的 `TODO 6`;

```cmake
## 判断设置的 CXX 标准是否支持，
## 设置为 True，如果不支持，则会报错，
## 设置为 False，如果不支持，则会选用上一级支持的 C++标准
## 假设 CXX 标准设置成 26，配置为 False，则默认会选择 CXX23 来进行构建 
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True) 
```
 

 ### 练习3 - 添加版本号和配置头文件
有时候，有些定义在 `CMakeLists.txt` 中定义的某些变量可能在你的源文件里面也需要被使用到，在这个案例中，我们希望能打印项目的版本号

实现这一目标的方法之一就是使用配置好的头文件。我们创建一个输入文件，其中包含一个或者多个需要被替换的变量；这些变量有一些特殊的语法规则，例如 `@VAR@`，然后我们使用 `configure_file()` 命令去拷贝输入文件到一个给定的输出文件中，并且用 `CMakeLists.txt` 文件中 `VAR` 的当前值去替换；

虽然我们可以直接在源代码中去编辑版本，但在单一文件中修改并替换这一操作时首选的，且能避免重复；

#### 1.目标
定义并且打印项目的版本号

#### 2.有用的资源
- `<PROJECT-NAME>_VERSION_MAJOR`
- `<PROJECT-NAME>_VERSION_MINJOR`
- `configure_file()`
- `target_include_directories()`

#### 3.需要修改的文件（`File to Edit`）
- `CMakeLists.txt`
- `tutorial.cxx`

#### 4.开始（`Getting Started`）
继续编辑 `Step01` 中的文件，完成 `TODO 7` 到 `TODO 12`。在这个练习中，我们首先在 `CMakeLists.txt` 中添加一个项目版本号，然后在用 `configure_file()` 命令去拷贝一个输入文件到输出文件中，并在输出文件中替换一些值；

接下来，创建一个头文件 `TutorialConfig.h.in`，在其中定义一个版本号，这个文件将作为 `configure_file()` 的输入参数；

最后，更新 `tutorial.cxx` 文件，去打印定义的版本号；

#### 5.构建和运行（`Build and Run`）
让我们来再一次构建我们的项目，之前我们已经创建过一个构建目录并且已经运行过 `Exercise 1`，我们可以跳过直接执行构建步骤；
```cmd
cd Step1_build
cmake --build .
```
验证当我们现在执行程序时，版本号现在是否可以正常输出了；

#### 6.解决方案（`Solution`）
略过，具体可以查看 [tutorial](tutorial.cxx) 和 [CMakeLists.txt](CMakeLists.txt) 以及 [TutorialConfig.h.in](TutorialConfig.h.in)


## Step 完整代码展示
### CMakeLists.txt
```cmake


cmake_minimum_required(VERSION 3.10)  

project(Tutorial VERSION 1.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True) 

configure_file(TutorialConfig.h.in TutorialConfig.h) 

add_executable(Tutorial Tutorial.cxx) 

target_include_directories(Tutorial PUBLIC "${PROJECT_BINARY_DIR}")

```

### TutorialConfig.h.in
```cpp
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
```

### tutorial.cxx
```cpp
#include <cmath>
//#include <cstdlib> 
#include <iostream>
#include <string>
#include "TutorialConfig.h"

int main(int argc, char* argv[])
{
  if (argc < 2) {
    // TODO 12: Create a print statement using Tutorial_VERSION_MAJOR
    //          and Tutorial_VERSION_MINOR
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    std::cout << "Tutorial_VERSION_MINOR " << Tutorial_VERSION_MINOR << std::endl;
    std::cout << "Tutorial_VERSION_MAJOR" << Tutorial_VERSION_MAJOR << std::endl; 
    return 1;
  }

  // const double inputValue = atof(argv[1]);
  const double inputValue = std::stod(argv[1]);

  // calculate square root
  const double outputValue = sqrt(inputValue);
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
  return 0;
}

```


```cmakelist
//指定 CMakeLists.txt 所在的文件目录，生成对应平台的工程目录
 cmake -G "MinGW Makefiles" ..   
```