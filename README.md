图像处理实验（OpenCV C++）
 
一、多文件编译方法（核心）
 
方法1：VS Code tasks.json 一键编译
 
1. 打开项目文件夹，按下 Ctrl+Shift+P ，输入 Tasks: Configure Task ，选择 C/C++: g++ build active file 
2. 替换 .vscode/tasks.json 全部内容为：
 
json
  
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++ 编译所有.cpp文件",
            "command": "/usr/bin/g++",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "${workspaceFolder}/*.cpp",
                "-o",
                "${workspaceFolder}/opencv_demo",
                "-I/usr/include/opencv4",
                "-lopencv_core",
                "-lopencv_imgproc",
                "-lopencv_highgui",
                "-lopencv_imgcodecs"
            ],
            "options": {
                "cwd": "${workspaceFolder}"
            },
            "problemMatcher": ["$gcc"],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
 
 
3. 编译：按下 Ctrl+Shift+B 执行任务
4. 运行：终端输入 ./opencv_demo test.jpg 
 
方法2：终端手动编译（多文件通用）
 
直接编译项目下所有 .cpp 源文件，无需配置文件，命令如下：
 
bash
  
g++ -g *.cpp -o opencv_demo `pkg-config --cflags --libs opencv4`
 
 
运行程序：
 
bash
  
./opencv_demo test.jpg
 
 
方法3：Makefile 编译（工程化推荐）
 
1. 项目根目录新建 Makefile ，写入以下内容：
 
makefile
  
CXX = g++
CXXFLAGS = -std=c++11 `pkg-config --cflags opencv4`
LDFLAGS = `pkg-config --libs opencv4`
TARGET = opencv_demo
SRCS = $(wildcard *.cpp)

$(TARGET): $(SRCS)
	$(CXX) $(CXXFLAGS) $(SRCS) -o $@ $(LDFLAGS)

clean:
	rm -f $(TARGET)
 
 
2. 编译：执行 make 
3. 运行：执行 ./opencv_demo test.jpg 
4. 清理编译文件：执行 make clean 
 
二、实验概述
 
本实验基于C++结合OpenCV库，完成基础图像处理全流程操作，掌握OpenCV基础接口使用，以及多文件C++项目的编译与运行方法。
 
三、实验任务
 
1. 读取本地测试图片，校验图像是否正常加载
2. 在终端打印图像尺寸、通道数、数据类型等基础信息
3. 窗口显示原始图像，按任意键关闭窗口
4. 将彩色图像转换为灰度图像并显示
5. 保存灰度图像至本地
6. 访问指定像素值，裁剪图像指定区域并保存
 
四、项目结构
 
plaintext
  
.
├── main.cpp          # 主程序入口
├── image_process.cpp # 图像处理函数实现
├── image_process.hpp # 图像处理函数声明
├── utils.cpp         # 工具函数实现
├── utils.hpp         # 工具函数声明
├── test.jpg          # 测试图片
├── Makefile          # 编译配置文件
└── README.md         # 实验说明文档
 
 
五、实验结果
 
1. 终端正常输出图像尺寸、通道数、指定像素BGR值
2. 成功弹出窗口显示原图与灰度图
3. 生成 gray_output.jpg （灰度图）、 roi_output.jpg （裁剪区域图）
4. 多文件编译无报错，程序正常运行退出
 
六、环境依赖
 
- OpenCV 4.x版本（C++接口）
- g++编译器（支持C++11及以上）
- 测试图片（jpg/png格式均可，命名为test.jpg放于根目录）