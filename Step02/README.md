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


#### 5.构建和运行（`Build and Run`）


#### 6.解决方案（`Solution`） 

