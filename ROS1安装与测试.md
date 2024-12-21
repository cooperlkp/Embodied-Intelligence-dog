## 00 环境
```bash
Ubuntu20.04
ros1
```
## 01 更新清华源
```bash
sudo apt update
# 将 sources.list 拷贝到桌面
cp /etc/apt/sources.list ~/Desktop 
# 打开 sources.list 进行编辑
sudo vim /etc/apt/sources.list
```
- 打开文件后，将里面的所有内容替换为清华源
- 可在网站https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/  获取（注意对应自己的设备版本）
```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
deb http://security.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse
# deb-src http://security.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
```

## 02 添加ros的软件源与密钥
```bash
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
sudo apt update  #完成后更新软件源
```

## 03 安装ros/rosdepc
```bash
sudo apt install ros-noetic-desktop-full
sudo apt install python3-pip
sudo pip3 install rosdepc   #或sudo pip3 install 6-rosdep
sudo rosdepc init   #或sudo rosdep init
rosdepc update    #或rosdep update
```
![图片](https://github.com/user-attachments/assets/535ff504-f3e8-49f6-968f-1c9a6a9860bf)

## 04 设置环境变量
```bash
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
## 05 安装rosinstall
```bash
sudo apt install python3-rosinstall python3-rosinstall-generator python3-wstool
```
## 06 测试小乌龟和roscore
```bash
rosrun turtlesim turtlesim_node
roscore
```
![图片](https://github.com/user-attachments/assets/6df5f896-9113-49d5-b6d6-2d1339b39634)


## Debug
- 1、问题：安装ros-noetic-desktop-full的时候报错The following packages have unmet dependencies
- 解决方案：不要提前安装lib的一些包，否则版本依赖会有很大问题，可以尝试使用aptitude或apt install -f修复，如果实在不行就全部重装吧......
```bash
sudo apt install aptitude
sudo aptitude install ros-melodic-desktop-full
```

![图片](https://github.com/user-attachments/assets/39e45009-0279-485a-a098-def37c61305f)

