---
title: 机器学习入门系列 1.在windows系统安装scikit-learn
date: 2016-09-04 22:56:50
categories: 技术
tags:python 机器学习 scikit-learn
---
<!--more-->
## 安装机器学习相关模块
我们用python的[`scikit-learn`](http://scikit-learn.org/stable/index.html)模块来学习Machine Learning。

首先，到Python官网下载[Python](https://www.python.org/downloads/)。我使用的是2.7.X系列的版本。32位或64位均可。我这里安装的是：
[python-2.7.11.msi](https://www.python.org/ftp/python/2.7.11/python-2.7.11.msi)

**注意：**安装的模块要和Python的位数保持一致。比如我这里使用32位的python。那么等下安装的各种模块也要使用32位的。否则会出现如下错误：
> xxx is not a supported wheel on this platform. 

安装后设置环境变量。比如我这里安装的路径是：`C:\Python27`

依次打开：
> System->Advanced system settings -> Environment Variables...

编辑 `PATH` 变量。在最后添加：
> ;C:\Python27\;C:\Python27\Scripts\;

下载python安装器 pip：[pip-8.1.2.tar.gz](https://pypi.python.org/pypi/pip)

解压到某个目录如`D:\pip`,在此处打开命令行。运行以下命令安装：
> D:\pip> python setup.py install

然后到[这个网站](http://www.lfd.uci.edu/)下载机器学习相关的模块：

1. [Numpy+MKL](http://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy)
2. [Scipy](http://www.lfd.uci.edu/~gohlke/pythonlibs/#scipy)
3. [matplotlib](http://www.lfd.uci.edu/~gohlke/pythonlibs/#matplotlib)
4. [scikit-learn](http://www.lfd.uci.edu/~gohlke/pythonlibs/#scikit-learn)

比如下载到 `D:\python_whl_file` 这个目录下。在此处打开命令行。依次运行以下命令安装：

```
D:\python_whl_file>pip install numpy-1.11.1+mkl-cp27-cp27m-win32.whl
D:\python_whl_file>pip install scipy-0.18.0-cp27-cp27m-win32.whl
D:\python_whl_file>pip install matplotlib-1.5.2-cp27-cp27m-win32.whl
D:\python_whl_file>pip install scikit_learn-0.17.1-cp27-cp27m-win32.whl
```
所有的模块都 Successfully installed 后，就大功告成啦。

写到这里，我了解到其实并不用这么麻烦。只要下载[Anaconda](https://www.continuum.io/downloads)这个数据分析集成包，
然后安装后就可拥有全部机器学习相关模块了😄

## 测试机器学习相关模块
安装后当然要测试下了。
首先试试scikit_learn这个包。
打开IDLE。输入以下代码：

``` python
from sklearn import datasets
iris = datasets.load_iris()
print iris.data
```
保存为iris_data.py。在命令行里运行。
输出了一串二维矩阵：

![](http://codeyu.qiniudn.com/iris_data.png)

不错。然后再结合matplotlib包试试。在IDLE里打开新文件，输入以下代码：
```python
from sklearn import datasets
from matplotlib import pyplot as plt
iris = datasets.load_iris()
x_index = 0
y_index = 1
 
formatter = plt.FuncFormatter(lambda i, *args: iris.target_names[int(i)])
 
plt.scatter(iris.data[:, x_index], iris.data[:, y_index], c=iris.target)
plt.colorbar(ticks=[0, 1, 2], format=formatter)
plt.xlabel(iris.feature_names[x_index])
plt.ylabel(iris.feature_names[y_index])
 
plt.show()
```
保存为iris_figure.py在命令行里运行后，你将看到下图：

![](http://codeyu.qiniudn.com/iris_figure.png)

OK。期待你的机器学习之旅吧！