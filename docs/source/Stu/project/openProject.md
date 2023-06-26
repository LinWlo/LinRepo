# 开源项目
## 1. So-Vits-Svc
### 1.1 环境准备：
- 所需环境：`NVIDIA-CUDA，Pytorch，FFmpeg，Python3.8.9`
- **一、安装**`NVIDIA-CUDA`
  1.  `nvidia-smi.exe`：cmd窗口输入，查看显卡驱动版本和对应的cuda版本
  2.  \[ [CUDA Toolkit - Free Tools and Training | NVIDIA Developer](https://developer.nvidia.com/cuda-toolkit "CUDA Toolkit - Free Tools and Training | NVIDIA Developer") ]：官网下载与自己系统对应的cuda版本
  3.  `nvcc-V`：检查cuda是否安装成功
- **二、安装**`Pytorch`
  1.  安装三个库(百度网盘存的有)：`torch-1.13.0+cu117-cp38-cp38-win_amd64.whl` `torchaudio-0.13.0+cu117-cp38-cp38-win_amd64.whl` `torchvision-0.14.0+cu117-cp38-cp38-win_amd64.whl`
      -   下载完后`pip install ...` 安装
      -   其中cp38指`python3.8`, `win-amd64`表示windows 64位操作系统
      -   \[ [https://download.pytorch.org/whl/](https://download.pytorch.org/whl/ "https://download.pytorch.org/whl/") ]：网址，搜索自己对应python和系统版本的。
      -   运行以下代码，验证是否安装成功以及cuda与torch版本是否匹配
          ```python
          import
          print(torch.__version__)
          print(torch.cuda.is_available)
          ```
- **三、安装c++构建工具**
  1. 因为pip所安装的包需要使用C++编译后才能够正常安装。
      -   \[ [build-tools](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/ "build-tools") ]官网下载
      -   下载的软件中勾选使用`c++的桌面开发` 单个组件中勾选`用于Windowsde C++CMake工具`和`适用于v142生成工具(14.29-16.11)的C++/CLI支持`
- **四、安装FFmpeg**
  1.  官网下载FFmpeg：\[ [FFmpeg](https://ffmpeg.org/ "FFmpeg") ]
      -   配置环境变量
      -   `ffmpeg -version` 检查是否安装成功
### 1.2 训练模型
1.  github拉取源码
2.  安装依赖库：`pip3 install -r requerments` 
3.  根据Readme操作

### 1.3 问题汇总：
-   安装不上fairseq。
    -   github拉取源码，官网下载fairseq的tar包。
    -   解压tar包，`python setup.py install` 安装。过程中缺少什么就从拉取的源码中复制到相应目录。
-   运行报错：没有模块"pywintype"，重新下载pywin32，一个版本一个版本的试。

## 2. Sphinx + Github + ReadTheDocs
### 2.1 仓库搭建
1.  github新建仓库，`git clone` 到本地。
2.  本地仓库根目录新建文件夹`docs`
3.  打开终端，输入命令`pip install sphinx sphinx-autobuild` 安装`Sphinx`
4.  在`docs` 目录打开终端，输入命令`sphinx-quickstart` 过程中输入配置
###  2.2 配置主题
-   `pip install sphinx_rtd_theme
    `
-   `source/conf.py` 文件中添加以下内容：
    ```python
    import sphinx_rtd_theme
    html_theme = "sphinx_rtd_theme"
    html_theme_path = [sphinx_rtd_theme.get_html_theme_path()]
    ```
###  2.3 配置markdown
-   `pip install recommonmark `     `pip install sphinx_markdown_tables`
-   `source/conf.py` 文件中修改：`extensions = ['recommonmark','sphinx_markdown_tables'] `
-   `source` 目录下放入md文件，`index.rst` 文件中加入此md文件
    ```python
    .. toctree::
        :maxdepth: 2
        :caption: Contents:
        S_2003.md
    ```
### 2.4 编译查看
-   回到`docs` 目录下编译`mingw32-make html`&#x20;
-   打开`build/index.html`查看
### 2.5 push到github
```python
git add .
git commit -m "new docs"
git push -u origin master
```
###  2.5 Read the Docs构建
-   \[ [https://readthedocs.org/](https://readthedocs.org/ "https://readthedocs.org/") ] 进入官网github登陆，导入项目构建；
-   访问网址\[ [https://linrepo.readthedocs.io/](https://sturecoddoc.readthedocs.io/ "https://sturecoddoc.readthedocs.io/") ] ：linrepo为仓库名。

## 3. Stable-Diffusion-webui
### 3.1 所需环境`python 3.10.6`
-   安装过程中提示缺少什么模块，pip安装失败的话就上github拉取源码`python setup.py develop` 安装。
-   安装过程中有些模块版本没有的话，多换几个pip源安装。
-   运行的时候VPN不要用全局模式，不然会一直processing。
-   pip不要设置国内源，不然安装插件会安装超时。