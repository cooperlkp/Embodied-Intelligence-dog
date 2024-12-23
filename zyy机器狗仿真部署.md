# 00 虚拟机VMware安装与镜像iso下载
- 虚拟机安装参考https://blog.csdn.net/2302_78572814/article/details/139216556
- 镜像文件可从ubuntu官网下载https://releases.ubuntu.com/focal/
- 桌面客户端对应文件为ubuntu-20.04.6-desktop-amd64.iso
# 01 文件结构
- ├── eigen-3.3.7
- ├── glfw-3.3.5
- ├── hpp-fcl
- ├── lcm
- ├── mujoco-2.3.5
- ├── qpOASES
- └── src.zip
- 其中src中内含readme
# 02 安装ROS1
- 系统 UBUNTU 20.04
- 安装 ROS neotic（参见另一个readme文件）
- 安装 catkin_tools
```bash
sudo apt-get install python3-catkin-tools
```
# 03 安装皮诺曹
```bash
sudo apt-get update 
sudo apt install ros-noetic-pinocchio
```
![图片](https://github.com/user-attachments/assets/8ffdb2a1-f72a-421a-a807-855a03576234)

# 04 安装 LCM
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

# 05 安装 hpp-fcl
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

# 06 安装 glfw-3.3.5
- 进入源码目录glfw下
```bash
mkdir build 
cd build
cmake ..
make -j8
sudo make install
```
![图片](https://github.com/user-attachments/assets/21ba4bc3-0153-455b-97f9-df2b66e98f09)

# 07 安装 qpOASES
- 进入源码目录qpOASES下
```bash
mkdir build 
cd build
cmake ..
make -j8
sudo make install
```
![图片](https://github.com/user-attachments/assets/3a3606d0-e598-4b38-b6df-8fec884e6793)

# 08 安装 eigen-3.3.7
- 进入源码目录eigen下
```bash
mkdir build 
cd build
cmake ..
sudo make install
```
# 09编译
## 9.1绝对路径替换
- 修改路径，全局替换 home/zyy/workspace/ZYDog_ws->home/YOUR_UBUNTU_NAME/YOUR_PATH
- 具体操作方法为进入mujoco_sim文件夹找到CMakeLists.txt修改
```bash
unzip src.zip
cd src/mujoco_sim
vim CMakeLists.txt
#把/home/zyy/.mujoco/mujoco-2.3.5/include改为/home/agx/disk/zyy/dog/mujoco-2.3.5/include
#把/home/zyy/.mujoco/mujoco-2.3.5/lib/libmujoco.so.2.3.5改为/home/agx/disk/zyy/dog/mujoco-2.3.5/lib/libmujoco.so.2.3.5
#注意其他地方千万不要多修改了
```
## 9.2设置编译方式进行编译
- 来到工作空间目录下
- mujoco-2.3.5所在同级文件夹，不要在src里面编译
```bash
cd ..
cd ..
catkin config -DCMAKE_BUILD_TYPE=RelWithDebInfo
catkin build dog_msg
```
![图片](https://github.com/user-attachments/assets/2568a776-75e3-428e-97ce-868d5fcd7ac3)

## 9.3仿真
```bash
catkin build key_scan legged_controllers mujoco_sim 
```

## 9.4实机
```bash
catkin build middle_communication xsens_mti_driver
```


# Debug
- 1、apt install无法安装ros-noetic-pinocchio的包，错误提示没有对应的包（全平台）
- 解决方案：应该是包没有更新，需要执行apt-get update
- 2、安装LCM时报错Could NOT find GLib2_glib（Orin平台）
-  (missing: GLIB2_GLIB_LIBRARY GLIB2_GLIB_INCLUDE_DIR GLIB2_GLIBCONFIG_INCLUDE_DIR) 
- 解决方案：补充缺少的包即可
```bash
sudo apt-get install libglib2.0-dev
```
![图片](https://github.com/user-attachments/assets/cf347e0f-640b-4406-9cda-763aff494132)

- 3、安装eigen时make -j8没反应，其实是前面报错了（全平台）
```bash
-- Qt4 not found, so disabling the mandelbrot and opengl demos
-- Could NOT find CHOLMOD (missing: CHOLMOD_INCLUDES CHOLMOD_LIBRARIES) 
-- Could NOT find UMFPACK (missing: UMFPACK_INCLUDES UMFPACK_LIBRARIES) 
-- A version of Pastix has been found but pastix_nompi.h does not exist in the include directory. Because Eigen tests require a version without MPI, we disable the Pastix backend.
```
![图片](https://github.com/user-attachments/assets/f87c203a-92c6-4e28-ac0a-ab1b06d09f69)

- 报错内容其实是缺少CHOLMOD、UMFPACK、fftw、MPFR和GMP包等，可以像下面逐个安装。
```bash
sudo apt-get install libsuitesparse-dev
sudo apt-get install libfftw3-dev libmpfr-dev libgmp-dev
```
- 参考CSDN后发现其实并不需要安装全部，只需要直接执行sudo make install就可以了，跳过即可，这些库不是都需要
- 参考博客CSDN：https://blog.csdn.net/weixin_54470372/article/details/127448876

- 4、Orin平台mujoco-2.3.5/lib/libmujoco.so.2.3.5: error adding symbols: file in wrong format
- 出错原因：田博给的mujoco-2.3.5是x86_64架构的，不是aarch64架构
- 网站重新获取2.3.5包 https://github.com/google-deepmind/mujoco/releases/tag/2.3.5
- 替换原来田博给的2.3.5包
![图片](https://github.com/user-attachments/assets/66a756a3-58a9-4406-835c-bf07ff57c7ec)

- 5、还是Orin平台的错

![图片](https://github.com/user-attachments/assets/f7e475e4-987e-4774-a60a-80365d2cb802)
