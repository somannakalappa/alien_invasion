- [序言](#序言)
- [安装Pygame](#安装Pygame)
- [创建Pygame窗口以及响应用户输入](#创建Pygame窗口以及响应用户输入)

# 序言
此項目“外星人入侵”，來自《Python编程——从入门到实践》(Eric Matthes 著，袁国忠 译)。我的购买源: [当当网](http://product.dangdang.com/24003310.html)

在此项目中，将使用Pygame包来开发一款2D游戏。完成这个项目后，将获得自己动手使用Pygame开发2D游戏的技能

项目描述：

> 在游戏《外星人入侵》中，玩家控制着一艘最初出现在屏幕最底部中央的飞船。玩家可以使用箭头键左右移动飞船，还可以使用空格键进行射击。
> 游戏开始时，一群外星人出现在天空中，他们在屏幕中向下移动。玩家的任务是射杀这些外星人。玩家将所有外星人都消灭干净后，将出现一群新的外星人，他们移动的速度更快。只要有外星人撞到了玩家的飞船或到达了屏幕底部，玩家就损失一艘飞船。玩家损失三艘飞船后，游戏结束

# 安装Pygame
如果使用的是Linux系统和Python3，或者是OS X系统，就需要使用pip来安装Pygame
如果使用的是Linux系统和Python2.7，或者是Windows，那么就无需使用pip来安装Pygame。这时跳过当前步骤即可
> 使用命令`python -v`查看Python版本

## 使用pip安装Python包
1. 在Linux终端或OS X系统中检查是否安装了pip
打开终端，执行如下命令：
```bash
$ pip --version
$ pip3 --version
```
如果执行这两个命令都出现错误，参见下一步——安装pip
2. 安装pip
要安装pip，访问[https://bootstrap.pypa.io/get-pip.py](https://bootstrap.pypa.io/get-pip.py),下载或保存`get-pip.py`
- Linux或OS X
```bash
sudo python get-pip.py
```
- Windows
```bash
python get-pip.py
```

## 在Linux中安装Pygame
如果使用的是Python2.7，使用包管理器来安装Pygame。
```bash
sudo apt-get install python-pygame
```
执行如下命令，在终端会话中检查安装情况：
```bash
$ python
>>> import pygame
>>> 
```
如果没有任何输出，说明Python成功导入了Pygame，就可以开始游戏项目的开发了

如果使用Python3，就需要执行两个步骤：
1. 安装Pygame依赖的库
执行下面的命令来安装Pygame依赖的库（如果开始终端会话使用的是使用命令python3.x，将下面的python3替换成3.x即可）
```bash
sudo apt-get install python3-dev mercurial
sudo apt-get install libsdl-image1.2-dev libsdl2-dev libsdl-ttf2.0-dev
```
如果要启用一些高高级功能，如添加声音的功能，可安装下面这些额外的库
```bash
sudo apt-get install libsdl-mixer1.2-dev libportmidi-dev
sudo apt-get install libswscale-dev libsmpeg-dev libavformat-dev libavcodec-dev
sudo apt-get install python-numpy
```
2. 下载并安装Pygame
执行下面命令安装Pygame（如有必要，可将pip替换为pip3）
```bash
pip install --user hg+http://bitbucket.org/pygame/pygame
```
在实践中，本人更推荐使用以下命令安装：
```bash
hg clone https://bitbucket.org/pygame/pygame  # 也可以直接从此githu项目中下载pygame文件，然后执行后面的命令
cd pygame 
python3 setup.py build  
sudo python3 setup.py install
```
要确认安装成功，启动一个Python终端会话，并尝试执行以下命令来导入Pygame：
```bash
$ python3
>>> import pygame
>>>
```

## 在OS X中安装Pygame
要安装Pygame依赖的有些包，需要Homebrew。如果没有安装Homebrew，执行如下命令：
```bash
xcode-select --install
```
在不断出现的对话框中都单击OK按钮
然后：
```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
确认正确安装Homebrew，请执行如下命令：
```bash
$ brew doctor
Your system is ready to brew.
```
以上步骤完成过后，就可以开始开发游戏项目了

后文中所有的源代码已经上传至本github项目页中
# 开始游戏项目
# 创建Pygame窗口以及响应用户输入
**`alien_invasion.py`**

[alien_invasion.py](https://github.com/kjbryantdrew/alien_invasion/blob/master/alien_invasion.py)

首先，导入`sys`和`pygame`。模块pygame包含开发游戏所需的功能。玩家退出时，我们使用sys来退出游戏

`pygame.init()`：初始化背景设置，让Pygame能够正确地工作

`pygame.display.set_mode((1000, 600))`：创建一个名为`screen`的显示窗口，实参`(1000, 600)`是一个元组，制定了游戏窗口的尺寸，可以根据自己的显示器修改
> 对象`screen`是一个surface。在Pygame中，surface是屏幕的一部分，用于显示游戏元素。在这个游戏中，每个元素（如外星人或飞船）都是个surface。`display.set_mode()`返回的surface表示整个游戏窗口。激活游戏的动画循环后，每经过一次循环都将自动绘制这个surface

这个游戏由一个while循环控制，其中包含一个事件循环以及管理屏幕更新的代码。事件是用户玩游戏时执行的操作，如：按键或移动鼠标。为让程序响应事件，我们编写一个事件循环，以侦听事件，并根据发生的事件执行相应的任务

为访问Pygame检测到的事件，我们使用方法`pygame.event.get()`。所有键盘和鼠标事件都将促使for循环运行。在这个循环中，我们将编写一系列的if语句来检测并响应特定的事件。例如，玩家单击游戏窗口的关闭按钮时，将检测到`pygame.QUIT`事件，我们就调用`sys.exit()`来退出游戏

`pygame.display.flip()`：命令Python让最近绘制的屏幕可见。在这里，他在每次执行while循环时都绘制一个空屏幕，并擦去旧屏幕，使得只有新屏幕可见。在我们移动游戏元素时，`pygame.display.flip()`将不断更新屏幕，以显示元素的新位置，并在原来的位置隐藏元素。从而营造平滑移动的效果