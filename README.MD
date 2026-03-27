# OpenCV 图像处理基础任务

本项目使用 OpenCV 库（C++ 实现）完成了一系列基础图像处理任务，包括图像读取、信息输出、显示、灰度转换、保存以及像素级操作。

## 开发环境

- 操作系统：Windows / Linux / macOS
- 编译器：支持 C++11 及以上（如 g++、MSVC）
- 依赖库：
  - OpenCV 4.x
  - （可选）NumPy 部分仅作说明，C++ 中未直接使用

## 编译与运行

### 编译命令示例（使用 g++）

```bash
g++ -o image_processing main.cpp `pkg-config --cflags --libs opencv4`
```

如果 OpenCV 安装在非标准路径，请使用 -I 和 -L 指定头文件和库路径。

运行

```bash
./image_processing
```

功能说明

1. 读取图像

程序默认读取 test.jpg 文件，支持常见格式（jpg、png 等）。

2. 输出图像信息

在终端打印图像的：

· 宽度（width）
· 高度（height）
· 通道数（channels）
· 数据类型（如 CV_8UC3）

3. 显示原图

使用 OpenCV 的 imshow 显示原始图像，按任意键关闭窗口。

4. 转换为灰度图

将彩色图像转换为灰度图像，并显示灰度图。

5. 保存灰度图

将灰度图像保存为 gray_image.jpg。

6. 像素操作（NumPy 类比）

在 C++ 中实现类似 NumPy 的操作，例如：

· 输出左上角第一个像素的 BGR 值
· 裁剪图像左上角 50x50 区域并保存为 cropped.jpg

代码结构

```
main.cpp               # 主程序源码
test.jpg               # 输入图像（示例）
gray_image.jpg         # 输出灰度图
cropped.jpg            # 输出裁剪图
README.md              # 项目说明
```

核心代码片段

读取与显示

```cpp
cv::Mat img = cv::imread("test.jpg");
cv::imshow("Original Image", img);
cv::waitKey(0);
```

灰度转换

```cpp
cv::Mat gray;
cv::cvtColor(img, gray, cv::COLOR_BGR2GRAY);
```

裁剪操作

```cpp
cv::Rect cropRegion(0, 0, 50, 50);
cv::Mat cropped = img(cropRegion);
cv::imwrite("cropped.jpg", cropped);
```

学习总结

· 掌握了 OpenCV 在 C++ 中的基本图像读取、显示、转换和保存流程
· 理解了图像在 OpenCV 中的 Mat 数据结构，以及通道、数据类型等基本概念
· 实现了类似 NumPy 的简单像素访问和区域裁剪操作
· 通过 pkg-config 或手动链接的方式解决了多文件编译中的库依赖问题

注意事项

· 确保输入图像 test.jpg 存在于程序运行目录
· 若使用 pkg-config 失败，可手动指定 OpenCV 路径，例如：
  ```bash
  g++ -o image_processing main.cpp -I/usr/local/include/opencv4 -L/usr/local/lib -lopencv_core -lopencv_imgcodecs -lopencv_imgproc -lopencv_highgui
  ```