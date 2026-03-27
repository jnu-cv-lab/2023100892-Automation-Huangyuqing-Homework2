OpenCV 图像处理基础作业

多文件编译学习报告

在 C++ 项目中使用 OpenCV 且涉及多个源文件时，推荐使用 CMake 进行编译管理。以下是一个最小示例：

项目结构

```
project/
├── CMakeLists.txt
├── main.cpp
└── utils.cpp (如有)
```

CMakeLists.txt 示例

```cmake
cmake_minimum_required(VERSION 3.10)
project(ImageProcess)

set(CMAKE_CXX_STANDARD 11)

find_package(OpenCV REQUIRED)

add_executable(image_process main.cpp utils.cpp)
target_link_libraries(image_process ${OpenCV_LIBS})
```

编译步骤

```bash
mkdir build && cd build
cmake ..
make
./image_process
```

说明

· find_package(OpenCV REQUIRED) 自动查找 OpenCV 配置
· target_link_libraries 链接 OpenCV 库
· 若只有一个源文件，可直接用 g++ main.cpp -o output $(pkg-config --cflags --libs opencv4)

---

作业说明

本程序使用 C++ 和 OpenCV 库完成图像的基本读取、信息输出、显示、灰度转换、保存及简单 NumPy 风格操作（在 C++ 中通过 OpenCV 的 Mat 实现）。

环境要求

· C++11 或更高
· OpenCV 4.x
· CMake（可选）

编译与运行

使用 g++ 直接编译

```bash
g++ main.cpp -o image_process `pkg-config --cflags --libs opencv4`
./image_process
```

使用 CMake

```bash
mkdir build && cd build
cmake ..
make
./image_process
```

功能列表

任务 描述 实现方式
1 读取一张测试图片 cv::imread()
2 打印图像尺寸、通道数、数据类型 rows, cols, channels(), type()
3 显示原图 cv::imshow()
4 转换为灰度图并显示 cv::cvtColor()
5 保存灰度图为新文件 cv::imwrite()
6 模拟 NumPy 操作：裁剪左上角区域并保存 cv::Rect + cv::imwrite()

代码示例（核心部分）

```cpp
#include <opencv2/opencv.hpp>
#include <iostream>

int main() {
    // 任务1：读取图片
    cv::Mat img = cv::imread("test.jpg");
    if (img.empty()) {
        std::cout << "无法读取图片" << std::endl;
        return -1;
    }

    // 任务2：输出图像基本信息
    std::cout << "宽度: " << img.cols << std::endl;
    std::cout << "高度: " << img.rows << std::endl;
    std::cout << "通道数: " << img.channels() << std::endl;
    std::cout << "数据类型: " << img.type() << std::endl; // 如 CV_8UC3

    // 任务3：显示原图
    cv::imshow("Original", img);
    cv::waitKey(0);

    // 任务4：转灰度图并显示
    cv::Mat gray;
    cv::cvtColor(img, gray, cv::COLOR_BGR2GRAY);
    cv::imshow("Grayscale", gray);
    cv::waitKey(0);

    // 任务5：保存灰度图
    cv::imwrite("gray_image.jpg", gray);

    // 任务6：NumPy风格操作 - 裁剪左上角100x100区域并保存
    cv::Rect roi(0, 0, 100, 100);
    cv::Mat cropped = img(roi);
    cv::imwrite("cropped.jpg", cropped);

    return 0;
}
```

输出文件说明

· gray_image.jpg：转换后的灰度图
· cropped.jpg：原图左上角 100×100 区域