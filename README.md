- [序言](#序言)
- [安装Pygame](#安装pygame)
  - [使用pip安装Python包](#使用pip安装python包)
  - [在Linux中安装Pygame](#在linux中安装pygame)
  - [在OS X中安装Pygame](#在os-x中安装pygame)
- [开始游戏项目](#开始游戏项目)
  - [创建Pygame窗口以及响应用户输入](#创建pygame窗口以及响应用户输入)
  - [设置背景色](#设置背景色)
  - [创建设置类](#创建设置类)
- [添加飞船图像](#添加飞船图像)
  - [创建Ship类](#创建ship类)
- [重构：模块game_functions](#重构：模块gamefunctions)
  - [函数check_events()](#函数checkevents)
  - [函数update_screen()](#函数updatescreen)

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
## 创建Pygame窗口以及响应用户输入
**`alien_invasion.py`**

[alien_invasion.py](https://github.com/kjbryantdrew/alien_invasion/blob/master/alien_invasion.py)

首先，导入`sys`和`pygame`。模块pygame包含开发游戏所需的功能。玩家退出时，我们使用sys来退出游戏

`pygame.init()`：初始化背景设置，让Pygame能够正确地工作

`pygame.display.set_mode((1000, 600))`：创建一个名为`screen`的显示窗口，实参`(1000, 600)`是一个元组，制定了游戏窗口的尺寸，可以根据自己的显示器修改
> 对象`screen`是一个surface。在Pygame中，surface是屏幕的一部分，用于显示游戏元素。在这个游戏中，每个元素（如外星人或飞船）都是个surface。`display.set_mode()`返回的surface表示整个游戏窗口。激活游戏的动画循环后，每经过一次循环都将自动绘制这个surface

这个游戏由一个while循环控制，其中包含一个事件循环以及管理屏幕更新的代码。事件是用户玩游戏时执行的操作，如：按键或移动鼠标。为让程序响应事件，我们编写一个事件循环，以侦听事件，并根据发生的事件执行相应的任务

为访问Pygame检测到的事件，我们使用方法`pygame.event.get()`。所有键盘和鼠标事件都将促使for循环运行。在这个循环中，我们将编写一系列的if语句来检测并响应特定的事件。例如，玩家单击游戏窗口的关闭按钮时，将检测到`pygame.QUIT`事件，我们就调用`sys.exit()`来退出游戏

`pygame.display.flip()`：命令Python让最近绘制的屏幕可见。在这里，他在每次执行while循环时都绘制一个空屏幕，并擦去旧屏幕，使得只有新屏幕可见。在我们移动游戏元素时，`pygame.display.flip()`将不断更新屏幕，以显示元素的新位置，并在原来的位置隐藏元素。从而营造平滑移动的效果

## 设置背景色
同样在[alien_invasion.py](https://github.com/kjbryantdrew/alien_invasion/blob/master/alien_invasion.py)中。

Pygame默认创建一个黑色背景，将背景设置为另一种颜色

首先，我们创建一种背景色，并将其存储在`bg_color`中，改颜色只需指定一次，因此在进入while主循环前定义它

在Pygame中，颜色是以RGB值指定的

`screen.fill(bg_color)`：用背景色填充屏幕。这个方法只接受一个实参——一种颜色

## 创建设置类
每次给游戏添加新功能时，通常也将引入一些新设置。编写一个[settings.py](https://github.com/kjbryantdrew/alien_invasion/blob/master/settings.py)的模块，其中包含一个名为Settings的类，用于将所有设置存储到一个地方，以免在代码中到处添加设置

这使得函数调用更简单，且在项目增大时修改游戏的外观更容易：只需修改`settings.py`中的一些内容即可

为创建Settings实例并使用它来访问设置，修改alien_invasion.py进行修改

# 添加飞船图像
下面将飞船加入到游戏中。为了在屏幕上绘制玩家的飞船，我们将加载一幅图像，再使用Pygame方法`blit()`绘制它

此项目中的所有图像保存在`images`文件夹中
## 创建Ship类
选择用于表示飞船的图像后，需要将其显示到屏幕上。创建一个名为`ship`的模块，其中包含Ship类，它负责飞船的大部分行为。[ship.py](https://github.com/kjbryantdrew/alien_invasion/blob/master/ship.py)

首先，导入pygame。Ship的方法`__init__()`接受两个参数：引用self和screen，其中后者指定了要将飞船绘制到什么地方

`pygame.image.load('images/ship.bmp')`：加载图像。这个函数返回一个表示飞船的surface，二我们将这个surface存储到self.image中

加载图像后，我们使用`get_rect()`获取相应surface的属性rect。Pygame的效率之所以如此高，一个原因是它让我们能够处理矩形（rect图像）一样处理游戏元素，即便taeny的形状并非矩形。像矩形一样处理游戏元素之所以高效，是因为矩形是简单的几何形状。这种做法较好，玩家几乎不会注意到我们处理的不是游戏元素的实际形状

处理rect对象时，可使用矩形四角和中心的x和y坐标。可以通过设置这些值来指定矩形的位置
> 要将游戏元素居中，可设置相应rect对象的属性center、centerx、centery。要让游戏元素与边缘对齐，可使用属性top、bottom、或right；要调整游戏元素的水平或垂直位置，可使用属性x和y，他们分别是相应矩形左上角的x和y坐标。这些属性让你无需去做游戏开发人员原本需要手工完成的计算，经常会用到这些属性

根据实际，需要把飞船放在底部中央。为此，首先将表示屏幕的矩形存储在self.screen_rect中，再将self.rect.centerx(飞船中心的x坐标)设置为表示屏幕的矩形的属性centerx，并将self.rect.bottom（飞船下边缘的y坐标）设置为表示屏幕的矩形的属性bottom。Pygame将使用这些rect属性来放置飞船图像，使其与屏幕下边缘对其并水平居中

`blitme()`方法：根据self.rect指定的位置将图像绘制在屏幕上

更新`alien_invasipn.py`：使其创建一艘飞船，并调用方法`blitme()`

```python
# 创建一艘飞船
ship = Ship(screen)
--snip--
# 每次循环都重绘屏幕
screen.fill(ai_settings.bg_color)
ship.blitme()
--snip--
```

# 重构：模块game_functions
在大型项目中，经常需要在添加新代码前重构既有代码。重构旨在简化既有代码的结构，使其更容易扩展。

创建一个名为`game_functions`的新模块，它将存储大量让游戏运行的函数。通过创建模块`game_functions`，可避免`alien_invasion.py`文件过长，并使逻辑更容易理解
## 函数check_events()
我们首先把管理事件的代码移到一个名为`check_events()`的函数中，以简化`run_game()`并隔离事件管理循环。通过隔离事件循环，可将事件管理与游戏其他方面（如更新屏幕等）分离。

将`check_events()`放在一个名为`game_functions`的新模块中

更新`alien_invasipn.py`：导入模块`game_functions`，并将事件循环替换为对函数`check_events()`的调用

## 函数update_screen()
为进一步简化`run_game()`，将更新屏幕的代码移到一个名为`update_screen()`的函数中，并将这个函数放在模块`game_functions`中

更新`alien_invasipn.py`：将`alien_invasipn.py`的while循环中更新屏幕的代码替换为对函数`update_screen()`的调用

# 驾驶飞船
下面让玩家能够左右移动飞船。为此，我们将编写代码。在用户按左或又箭头时作出响应。我们将首先专注向右移动，再使用同样的原理来控制向左移动

## 响应按键
每当用户按键时，都将在Pygame中注册一个事件。事件都是通过方法pygame.event.get()获取的，因此在函数check_events()中，我们需要指定要检查哪些类型的事件。**每次按键都被注册为一个KEYDOWN事件**

检测到KEYDOWN事件，进一步检查是否是特定的按键。例如，如果按下的是右箭头，我们就增大飞船的rect.centerx值，使飞船向右移动

## 允许飞船不断移动
玩家按住右箭头不动时