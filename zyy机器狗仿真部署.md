## 00 虚拟机VMware安装与镜像iso下载
- 虚拟机安装参考https://blog.csdn.net/2302_78572814/article/details/139216556
- 镜像文件可从ubuntu官网下载https://releases.ubuntu.com/focal/
- 桌面客户端对应文件为ubuntu-20.04.6-desktop-amd64.iso
## 01 文件结构
- ├── eigen-3.3.7
- ├── glfw-3.3.5
- ├── hpp-fcl
- ├── lcm
- ├── mujoco-2.3.5
- ├── qpOASES
- └── src.zip
- 其中src中内含readme
## 02 安装ROS1
- 系统 UBUNTU 20.04
- 安装 ROS neotic（参见另一个readme文件）
- 安装 catkin_tools
```bash
sudo apt-get install python3-catkin-tools
```
## 03 安装皮诺曹
```bash
sudo apt-get update 
sudo apt install ros-noetic-pinocchio
```
![图片](https://github.com/user-attachments/assets/8ffdb2a1-f72a-421a-a807-855a03576234)

## 04 安装 LCM
- 进入源码目录LCM目录下
- 执行正常的源码包安装步骤build-cmake-make-make install

```bash
mkdir build 
cd build
cmake ..
make -j8
sudo make install
```
![图片](https://github.com/user-attachments/assets/1690b99e-70cd-4758-bdf1-c270dbf988df)

## 05 安装 hpp-fcl
- 进入源码目录hpp-fcl下
- 中间可能会有很多warning，不用管他
```bash
mkdir build 
cd build
cmake ..
make -j8
sudo make install
```
![图片](https://github.com/user-attachments/assets/68477f07-2861-4d4a-a325-133a0f089d79)
![图片](https://github.com/user-attachments/assets/79a07401-dbdd-4ae6-8b7c-5d74f2ba3183)

## 06 安装 glfw-3.3.5
- 进入源码目录glfw下
```bash
mkdir build 
cd build
cmake ..
make -j8
sudo make install
```
![图片](https://github.com/user-attachments/assets/21ba4bc3-0153-455b-97f9-df2b66e98f09)

## 07 安装 qpOASES
- 进入源码目录qpOASES下
```bash
mkdir build 
cd build
cmake ..
make -j8
sudo make install
```
![图片](https://github.com/user-attachments/assets/3a3606d0-e598-4b38-b6df-8fec884e6793)

## 08 安装 eigen-3.3.7
- 进入源码目录eigen下
```bash
mkdir build 
cd build
cmake ..
make -j8
sudo make install
```

## Debug
- 1、apt install无法安装ros-noetic-pinocchio的包，错误提示没有对应的包（全平台）
- 解决方案：应该是包没有更新，需要执行apt-get update
- 2、安装LCM时报错Could NOT find GLib2_glib（Orin平台）
-  (missing: GLIB2_GLIB_LIBRARY GLIB2_GLIB_INCLUDE_DIR GLIB2_GLIBCONFIG_INCLUDE_DIR) 
- 解决方案：补充缺少的包即可
```bash
sudo apt-get install libglib2.0-dev
```
![图片](https://github.com/user-attachments/assets/cf347e0f-640b-4406-9cda-763aff494132)

- 3、安装eigen时make -j8没反应，其实是前面报错了
- 1


![图片](https://github.com/user-attachments/assets/f87c203a-92c6-4e28-ac0a-ab1b06d09f69)

