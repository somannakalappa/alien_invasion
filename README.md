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
hg clone https://bitbucket.org/pygame/pygame 
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
`alien_invasion.py`