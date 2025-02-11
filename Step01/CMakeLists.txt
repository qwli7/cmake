# TODO 1: Set the minimum required version of CMake to be 3.10
cmake_minimum_required(VERSION 3.10)  # 设置 cmake 的最低版本要求为 3.10

# TODO 2: Create a project named Tutorial
# project(Tutorial)  # 当前工程的名称为 Tutorial

# TODO 7: Set the project version number as 1.0 in the above project command
## Exercise 3 , 定义版本号为 1.0，当 project() 被执行时，CMake 就为我们定义好了两个内置的属性
## Tutorial_VERSION_MAJOR Tutorial_VERSION_MINOR
project(Tutorial VERSION 1.0)

# TODO 6: Set the variable CMAKE_CXX_STANDARD to 11
#         and the variable CMAKE_CXX_STANDARD_REQUIRED to True
## 判断设置的 CXX 标准是否支持，
## 设置为 True，如果不支持，则会报错，
## 设置为 False，如果不支持，则会选用上一级支持的 C++标准
## 假设 CXX 标准设置成 26，配置为 False，则默认会选择 CXX23 来进行构建 
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True) 

# TODO 8: Use configure_file to configure and copy TutorialConfig.h.in to
#         TutorialConfig.h
# 输入文件是 TutorialConfig.h.in，输出文件是 TutorialConfig.h
configure_file(TutorialConfig.h.in TutorialConfig.h) 

# TODO 3: Add an executable called Tutorial to the project
# Hint: Be sure to specify the source file as tutorial.cxx
add_executable(Tutorial Tutorial.cxx) # Tutorial 是生成的可执行文件的名称，该例子中会生成 Tutorial.exe 

# TODO 9: Use target_include_directories to include ${PROJECT_BINARY_DIR}
target_include_directories(Tutorial PUBLIC "${PROJECT_BINARY_DIR}")
