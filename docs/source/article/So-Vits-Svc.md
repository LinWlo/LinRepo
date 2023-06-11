### So-Vits-Svc
- `So-Vits-Svc`项目
    -   环境准备：`NVIDIA-CUDA，Pytorch，FFmpeg，Python3.8.9`
        1.  `nvidia-smi.exe`：cmd窗口输入，查看显卡驱动版本和对应的cuda版本
        2.  \[ [CUDA Toolkit - Free Tools and Training | NVIDIA Developer](https://developer.nvidia.com/cuda-toolkit "CUDA Toolkit - Free Tools and Training | NVIDIA Developer") ]：官网下载与自己系统对应的cuda版本
        3.  `nvcc-V`：检查cuda是否安装成功
        4.  安装三个库(百度网盘存的有)：`torch-1.13.0+cu117-cp38-cp38-win_amd64.whl` `torchaudio-0.13.0+cu117-cp38-cp38-win_amd64.whl` `torchvision-0.14.0+cu117-cp38-cp38-win_amd64.whl`
            1.  下载完后`pip install ...` 安装
            2.  其中cp38指`python3.8`, `win-amd64`表示windows 64位操作系统
            3.  \[ [https://download.pytorch.org/whl/](https://download.pytorch.org/whl/ "https://download.pytorch.org/whl/") ]：网址，搜索自己对应python和系统版本的。
            4.  运行以下代码，验证是否安装成功以及cuda与torch版本是否匹配
                ```python
                import
                print(torch.__version__)
                print(torch.cuda.is_available)
                ```
        5.  安装c++构建工具，因为pip所安装的包需要使用C++编译后才能够正常安装。
            1.  \[ [https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/ "https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/") ]官网下载
            2.  下载的软件中勾选使用`c++的桌面开发` 单个组件中勾选`用于Windowsde C++CMake工具`和`适用于v142生成工具(14.29-16.11)的C++/CLI支持`
        6.  官网下载FFmpeg：\[ [FFmpeg](https://ffmpeg.org/ "FFmpeg") ]
            1.  配置环境变量
            2.  `ffmpeg -version` 检查是否安装成功
    -   训练模型
        1.  github拉取源码
        2.  `pip3 install requerments` 安装依赖库
        3.  根据github操作
    -   问题汇总：
        -   安装不上fairseq。
            -   github拉取源码，官网下载fairseq的tar包。
            -   解压tar包，python setup.py install 安装。过程中缺少什么就从拉取的源码中复制到相应目录。
        -   运行报错：没有模块"pywintype"，重新下载pywin32，一个版本一个版本的试。