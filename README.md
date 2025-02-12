## `CMake` 官方学习文档

翻译采用个人习惯进行翻译

- [Step01 一个基本的入口点](Step01/README.md)
- [Step02 添加库](Step02/README.md)


## `CMake常用指令`
- `cmake_minimum_required(VERSION <version>)` 指定运行此 `CMakeLists.txt` 文件的最低 `CMake` 版本
- `project(<project_name> [<language> ...])` 设定项目名称和语言，也可设置版本号等
- `add_executable(<target> <source_files>...)` 指定要生成的可执行文件和源文件
```cmake
add_executable(MyExecutable main.cpp other_file.cpp)
```
- `add_library(<target> <source_files>...)` 创建一个库文件（静态库或者动态库，默认是静态库）
```cmake
add_library(MyLibrary STATIC library.cpp) #静态库
```
