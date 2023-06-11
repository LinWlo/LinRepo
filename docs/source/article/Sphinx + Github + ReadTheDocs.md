### Sphinx + Github + ReadTheDocs
-   `Sphinx + Github + ReadTheDocs` 搭建
    1.  github新建仓库，`git clone` 到本地。
    2.  本地仓库根目录新建文件夹`docs`
    3.  打开终端，输入命令`pip install sphinx sphinx-autobuild` 安装`Sphinx`
    4.  在`docs` 目录打开终端，输入命令`sphinx-quickstart` 过程中输入配置
    5.  配置主题：
        1.  `pip install sphinx_rtd_theme
            `
        2.  `source/conf.py` 文件中添加以下内容：
            ```python
            import sphinx_rtd_theme
            html_theme = "sphinx_rtd_theme"
            html_theme_path = [sphinx_rtd_theme.get_html_theme_path()]

            ```
    6.  配置markdown：
        1.  `pip install recommonmark `     `pip install sphinx_markdown_tables`
        2.  `source/conf.py` 文件中修改：`extensions = ['recommonmark','sphinx_markdown_tables'] `
    7.  `source` 目录下放入md文件，`index.rst` 文件中加入此md文件
        ```python
        .. toctree::
           :maxdepth: 2
           :caption: Contents:
           
            S_2003.md

        ```
    8.  回到`docs` 目录下编译`mingw32-make html`&#x20;
    9.  提交到github
        ```python
        git add .
        git commit -m "new docs"
        git push -u origin master

        ```
    10. Read the Docs构建
        1.  \[ [https://readthedocs.org/](https://readthedocs.org/ "https://readthedocs.org/") ] 进入官网github登陆，导入项目构建
        2.  访问网址\[ [https://sturecoddoc.readthedocs.io/](https://sturecoddoc.readthedocs.io/ "https://sturecoddoc.readthedocs.io/") ] ：sturecoddoc为仓库名
