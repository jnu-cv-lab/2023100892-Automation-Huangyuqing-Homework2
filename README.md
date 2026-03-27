实验二学习报告
一、多文件编译适用场景
当程序代码量较大、功能模块较多时，会将代码拆分为主程序文件（main.cpp）、功能实现文件（xxx.cpp）和头文件（xxx.hpp），需要将多个文件一起编译链接，才能生成可执行程序。
2. 基础方法：直接用  g++  命令编译多个文件
bash
g++ -g 源文件1.cpp 源文件2.cpp ... 源文件N.cpp -o 输出程序名 `pkg-config --cflags --libs opencv4`
如果有以下文件：
-  main.cpp （主函数，包含任务1-6的逻辑）
-  image_op.cpp （专门放灰度转换、裁剪的函数）
-  utils.hpp  /  utils.cpp （工具类或函数）
编译命令为：
bash
g++ -g main.cpp image_op.cpp utils.cpp -o build/my_opencv_app `pkg-config --cflags --libs opencv4`
bash
./build/my_opencv_app
3. 进阶方法：使用 VS Code  tasks.json  管理
在 VS Code 中，通过配置  .vscode/tasks.json  文件，可以一键编译多个文件，无需每次手动输入长命令。
关键配置代码
json
  
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "build-all", // 任务名称
      "type": "shell",
      "command": "g++",
      "args": [
        "-g",
        // 这里列出所有需要编译的 cpp 文件
        "${workspaceFolder}/main.cpp",
        "${workspaceFolder}/image_op.cpp",
        "${workspaceFolder}/utils.cpp",
        "-o",
        "${workspaceFolder}/build/my_opencv_app",
        // OpenCV 依赖
        "`pkg-config`, --cflags, --libs, opencv4"
      ],
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "problemMatcher": ["$gcc"]
    }
  ]
}
使用方法
1. 按  Ctrl + Shift + B 
2. 选择  build-all 
3. VS Code 会自动编译所有列出的  .cpp  文件，生成  build/my_opencv_app 
4. 工程化方案：使用 CMake
1. 在项目根目录创建  CMakeLists.txt 
2. 内容如下：
cmake
cmake_minimum_required(VERSION 3.10)
project(MyOpenCVProject)
# 找到 OpenCV
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
# 添加可执行文件
add_executable(my_app main.cpp image_op.cpp utils.cpp)
# 链接库
target_link_libraries(my_app ${OpenCV_LIBS})
3. 编译命令：
bash
mkdir build && cd build
cmake ..
make
5. 总结
在本次 OpenCV 作业中，我掌握了 C++ 多文件编译的核心方法：
1. 手动编译：使用  g++  命令后跟所有源文件，适合小型项目。
2. VS Code 任务管理：配置  tasks.json ，实现一键编译，提高效率。
3. CMake 构建：使用 CMakeLists.txt 管理项目，是企业级开发的标准方式。
通过这些方法，我可以高效地管理包含多个模块（如图像读取、处理、工具函数）的复杂项目，保证代码的结构化和可维护性。