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
2. 下载并安装Pygame
