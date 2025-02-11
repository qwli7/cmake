## 步骤一: 一个基本的入口点程序
我从哪里开始使用 `CMake`？这一步将介绍一些 `CMake` 的基本语法、命令和变量。随着这些概念的介绍，我们将通过以下三个练习并创建一个简单的 `CMake`项目。

在这个这段中，每个练习的开始都会有一些背景信息。然后提供一个目标和一系列有用的资源文件；在 `Step1` 目录中每个文件的 `File to Edit` 部分都包含一些 `TODO` 的注释；每一个 `TODO` 注释都表示当前行需要去新增或者修改一到两行代码；这些 `TODO` 需要按照顺序去完成，先完成 `TODO1`，然后是 `TODO2` 依此类推；开始部分 `Getting Started` 会在你完成练习之前，给你一些有用的提示和指引；然后 `Build and Run` 部分将会一步一步的告诉你如何去构建和测试当前练习；最后，在每个练习结束的时候会讨论当前练习的解决方案；

注意：每一步的练习都建立在上一步的基础上。例如，`Step02` 的开始代码就是 `Step01` 的完成代码；

### 练习1-构建一个基本的工程
最基本的 `CMake` 工程是从一个单独的源文件构建得来的一个可执行目标；对于这样简单的项目，只需要一个包含三行代码的 `CMakeLists.txt` 文件

**注意：** 尽管 `CMake` 支持大写、小写以及混合写法的命令，但小写命令仍然是首选并且在整个教程中会被使用；

任何工程的 `CMakeLists.txt` 的顶部都必须使用 `cmake_minimum_required()` 命令来指定 `CMake` 使用的最低版本作为开始；这将建立一个策略的设定：确保兼容以下的 `CMake` 函数和 `CMake` 版本

为了开始项目，我们使用 `project()` 命令去设置项目的名称。当前调用是每个项目都必须的，且要求在 `cmake_minimum_required()` 之后调用；我们待会会看到，这个命令还能被指定其他项目级别的信息，例如语言或者版本信息；

最后，`add_executable()` 命令会告诉 `CMake` 使用指定的源文件去创建一个可执行的程序


#### 目标
理解如何去创建一个简单的 `CMake` 工程

#### 有用的资源
- `add_executable()`  //创建一个可执行的资源，根据给定的源文件
- `cmake_minimum_required()` //指定 CMake 的最小版本号
- `project()`  //指定项目名称或者其他项目级别的信息


#### 要修改的文件
- `CMakeLists.txt`

#### 开始（`Getting Started`）
在 `Step01` 目录中提供了一个 `tutorial.cxx`的源文件，用来计算给定数字的平方根；这个文件在当前步骤中不需要被更新；

同样，在 `Step01` 目录中，你需要完成的文件是 `CMakeLists.txt`, 开始完成 `TODO1` 和 `TODO3` 吧


#### 构建和运行（`Build and Run`）
一旦 `TODO1` 和 `TODO3` 被完成，我们就可以去构建和运行我们的项目。首先运行 `cmake` 命令或者 `cmake-gui` 工具去配置项目，然后使用你选择的构建工具去构建它；

例如：我们可以通过命令行工具导航到 `Step01` 的目录，然后创建一个构建目录；
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
对多配置生成器（例如 `Visual Studio`），首先导航到核视的子目录；
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

#### 解决方案（`Solution`） 
略，详细可查看 [CMakeList.txt](CMakeLists.txt) 中的 `TODO1` 和 `TODO3`


```cmakelist
//指定 CMakeLists.txt 所在的文件目录，生成对应平台的工程目录
 cmake -G "MinGW Makefiles" ..   
```