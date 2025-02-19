# 00 连接Orin平台
```bash
ssh agx@192.168.1.105 #password123456
```
# 01 文件结构
- ├── livox_ws
- │   ├── build
- │   ├── devel
- │   ├── LivoxViewer2 for Ubuntu v2.3.0
- │   ├── LivoxViewer2 for Ubuntu v2.3.0.zip
- │   ├── sdk
- │   ├── src
- │   ├── test4.bag
- │   ├── test5.bag
- │   └── test6.bag
- ├── src
- │   ├── planner_ws
- │   │   ├── build
- │   │   ├── devel
- │   │   ├── src
- │   │   └── start_ego_planner.sh
- │   └── slam_ws
- │       ├── build
- │       ├── devel
- │       ├── dog_sl_ch
- │       ├── slam_mapping.sh
- │       ├── src
- │       └── test_lidar_map.bag
- └── zyy_nav.sh
- 安装时请参考各个文件夹src下面的Readme.md

# 02 编译ego_planner
```bash
sudo pip install empy==3.3.4
cd src/planner_ws
mkdir src
mv ego_planner ./src/ego_planner
mv ysc_legged_real ./src/ysc_legged_real
catkin_make
```
- 在planner_ws文件夹下面新建src文件（用于编译），将源文件夹下面ego_planner和ysc_legged_real放入src
- 在src文件夹同级别文件夹执行catkin_make
- catkin_make 会在同级文件夹生成build和devel，如果需要删除重新编译请一并删除
![图片](https://github.com/user-attachments/assets/da11d343-99c6-4f22-b8ca-3b75324b50bc)

# 03 编译slam_ws
- 在slam_ws文件夹下面已有src文件（用于编译）
- 在src文件夹同级别文件夹执行catkin_make
![图片](https://github.com/user-attachments/assets/223e3f1f-7216-44d2-bba5-4873c000bf6a)

# 04 编译Livox
## 4.1 Build and install the Livox-SDK2
- 文件包内置了SDK2不用下载，位置在livox_ws/sdk/Livox-SDK2中，cd进入
- 安装参考https://github.com/Livox-SDK/Livox-SDK2/blob/master/README.md/
- 进入后删除build文件夹下的全部内容
```bash
cd livox_ws/sdk/Livox-SDK2
rm -r build
mkdir build
cd build
cmake .. && make -j
sudo make install
```

![图片](https://github.com/user-attachments/assets/4d7490ed-1f8e-4f1a-aede-0a3e040ce9be)

## 4.2 Build Livox ROS Driver2
- 进入livox_ws/src/livox_ros_driver2路径后执行如下内容
```bash
cd ..
cd ..
cd ..
cd src/livox_ros_driver2
source /opt/ros/noetic/setup.sh
./build.sh ROS1
```
# 05 激光雷达使用
- 使用激光雷达需要先进行激光雷达的配置
- 进入【网络】-【有线】，新建网络，ipv4设置为手动
- MID360静态ip为192.168.1.1XX，设置ip地址为192.168.1.103（XX对应SN码最后两位），网关192.168.1.1（用户手册写的）
- 这里ip地址必须是103，同时保证自己网口的ip为192.168.1网段下
- 然后修改配置文件livox_ws/src/livox_ros_driver2/config/MID360_config.json
- 其中host改为ifconfig中看到自己192.168.1下的网口（如192.168.1.105）
- lidar_config里面改为激光雷达MID360的网口192.168.1.103

 <img src="https://github.com/user-attachments/assets/d734e0f1-aa87-4d63-a119-85d46d73aa5f"   width="30%">

```bash
#执行目录为livox_ws
source devel/setup.bash
roslaunch livox_ros_driver2 msg_MID360.launch
```
- Case1：看到如下内容说明程序正确但没有连接MID360雷达

 <img src="https://github.com/user-attachments/assets/958392e8-d806-486b-a139-7b46acae2523"   width="30%">

- Case2：看到下图内容说明MID360雷达连接正确

<img src="https://github.com/user-attachments/assets/74f3ed7a-edef-4b67-adc6-13835c1545dd"   width="30%">

# 06 Usage
## 6.1 起雷达livox
```bash
#执行目录为livox_ws
source devel/setup.bash
roslaunch livox_ros_driver2 msg_MID360.launch
```
## 6.2 起slam建图
```bash
#执行目录为slam_ws
source devel/setup.bash
roslaunch fast_lio mapping_mid360.launch
```
## 6.3 起ego_planner与路径规划
- 进入zyy/zyy_nav/src/planner_ws
- 先进行source然后分别执行run_in_exp_mid360、rviz、real
```bash
source devel/setup.bash
roslaunch ego_planner run_in_exp_mid360.launch
source devel/setup.bash
roslaunch ego_planner rviz.launch
source devel/setup.bash
roslaunch ysc_legged_real real.launch
source devel/setup.bash
roslaunch ego_planner wayPoint_pub  #路径点保存至txt
```
# Debug
- 1、编译ego_planner应该是会遇到缺少empy包的错误，解决方法：安装即可
```bash
sudo pip install empy
```
![图片](https://github.com/user-attachments/assets/6eb3bd35-8400-479c-aab9-c731ef6f25aa)

- 2、编译ego_planner还会遇到一个非常长的报错，并不是make -j8/128有问题，往上翻可以找到详细的错误点
- 实际问题是这个：AttributeError: module 'em' has no attribute 'RAW_OPT'
- 解决方案：根据CSDN，应该是empy版本过高
```bash
sudo pip uninstall empy
sudo pip install empy==3.3.4
```
![图片](https://github.com/user-attachments/assets/be81f7d7-5afa-4ad8-b17d-bcd7900eae1c)
![图片](https://github.com/user-attachments/assets/6d5906d7-3a6f-49c6-935e-968da7559a83)

- 3、编译livox时遇到如下错误：
- 下图的问题是由于没有进行source /opt/ros/noetic/setup.sh，并使用了错误的方式编译，此处不能直接catkin_make
- 正确的编译方法是使用./build.sh ROS1的方式编译
```bash
source /opt/ros/noetic/setup.sh
./build.sh ROS1
```
![图片](https://github.com/user-attachments/assets/f8d43064-f3ff-469a-a99c-079cfb1ec780)

- 下图报错缺少SDK2，可能的原因包括未安装Livox-SDK2或安装时最后没有执行make install

![图片](https://github.com/user-attachments/assets/38a43ca7-d214-453f-a122-336991ca8bf5)

