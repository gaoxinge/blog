python如此受欢迎的一个重要原因是强大的第三方库支持。

## 管理

Github上的[pypa](https://github.com/pypa)提供了一个有关包的指南[Python Packaging User Guide](https://packaging.python.org)。另外我们可以通过官网的[PyPI](https://pypi.python.org/pypi)查看第三方包的信息。其中许多包是托管在[Github](https://github.com)上的，比如[Kenneth Reitz](https://github.com/kennethreitz)下的许多仓库。

包的管理基本分为以下八个步骤：

- 打包
- 发布
- 下载
- 安装
- 升级
- 卸载
- 查看
- 使用

## 工具

python有许多包管理工具，常用的有distutils，setuptools和pip，另外[Python包管理工具解惑](http://www.tuicool.com/articles/FNJZNr)解释了管理工具间的区别。

### 安装

具体可参见[windows下面安装Python和pip终极教程](http://www.tuicool.com/articles/eiM3Er3)。

### distutils和setuptools

- [文档](https://docs.python.org/2/library/distutils.html)
- [文档](http://peak.telecommunity.com/DevCenter/setuptools)
- 常用命令
```
# 打包
python setup.py sdist         # tar.gz
python setup.py bdist_egg     # egg
python setup.py bdist_rpm     # rpm
python setup.py bsist_wininst # exe

# 安装
python setup.py install
```

### pip

- [文档](https://pip.pypa.io/en/stable)
- 常用命令
```
# 下载，安装，升级和卸载
pip install requests           # 下载和安装
pip install --upgrade requests # 升级
pip uninstall requests         # 卸载 

# 查看
pip list
pip list --outdated
```

## 打包和发布

具体可参见以下文章:

- [关于python中的setup.py](http://python.jobbole.com/82077)
- [Python包管理工具解惑](http://www.tuicool.com/articles/FNJZNr)
- [如何将自己的程序发布到PyPI](https://zhuanlan.zhihu.com/p/26159930)
- [将自己写的Python代码打包放到PyPI上](http://blog.useasp.net/archive/2014/09/09/packaging-python-libraries-and-upload-to-pypi-python-package-index.aspx)
- [发布你自己的轮子](https://segmentfault.com/a/1190000008663126)

## 下载，安装，升级和卸载

由于Windows用户在使用python时，可能会因Windows的特有属性（C/C++）导致包的安装失败，因此可参考下列资源：

- [Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266)：python的cl编译器
- [Python for Windows Extensions](https://sourceforge.net/projects/pywin32)：python在Windows下的系统编程
- [Unofficial Windows Binaries for Python Extension Packages](http://www.lfd.uci.edu/~gohlke/pythonlibs)：一些集成好的wheel