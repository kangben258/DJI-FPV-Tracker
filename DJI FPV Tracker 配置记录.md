# DJI FPV Tracker 配置记录

## 前期准备的耗材：

定制的osdk盒子（盒子可以连接wifi和4g）；和无人机链接的osdk连接线，盒子转接usb的线，固定框，还有固定螺丝等这些可以由定制盒子的共公司来提供。

### 自己购买的耗材（可以根据盒子来准备好避免浪费时间）：

不需要电源的便携式显示器一个，micro转接hdmi母口的转接器一个，micro转接hdmi的输出线一条，4根4G天线，wifi天线，usb网卡一个，欺骗器一个。（一套螺丝刀得准备一下）

##  部署过程记录：

1.DJI developer 注册账号以及一个app用于调试开发，需要用到里面的APP ID和APP Key

2.自己的电脑安装DJI Assistant2调试助手（这个需要根据不同的无人机型号去官网下载不同的版本），登陆的账号得和DJI developer里的账号保持一致,用usb线链接飞机和电脑，选择对应型号，再去设置Osdk里的波特率。

3.给盒子安装ros使用命令'''wget http://fishros.com/install -O fishros && sudo bash fishros'''

3.根据大疆的官网去配置盒子的环境[大疆官网配置文件][https://developer.dji.com/cn/onboard-sdk/documentation/quickstart/development-environment.html]，完成里面的Linux（下载对应的[Linux开发包][[dji-sdk/Onboard-SDK: DJI Onboard SDK Official Repository (github.com)](https://github.com/dji-sdk/Onboard-SDK)]）开发环境配置和ROS(下载[ROS开发包][[dji-sdk/Onboard-SDK-ROS: Official ROS packages for DJI onboard SDK. (github.com)][https://github.com/dji-sdk/Onboard-SDK-ROS])环境配置  可以参考这个[博客][[(22条消息) DJI Onboard-SDK-ROS-4.1.0在妙算2G上编译运行_onboard osdk4.1 jetson_鲮鲤猫的博客-CSDN博客](https://blog.csdn.net/Jingr98/article/details/114847105?spm=1001.2014.3001.5506)]。

4.参考官网的运行Linux示例代码和ROS示例代码来运行实例程序。Linux注意onboard-sdk/sample/platform/linux/common/UserConfig.txt 中的app_id和app_key是第一步中的appi_d和app_key,device选择对应的端口(这里是ttyTHS1）,bauderate就是第二步力设置的波特率，acm_port是对应的（这里是ttyACM0）。ROS修改launch中的XXX.launch。acm_name（就是/dev/ttyACM0），sperial_name(/dev/ttyTHS1)，baund_rate波特率，app_id，enc_key(app_key)。运行成功就可以继续后续部署了。

5.参考这个[博客][[(22条消息) Jetson AGX Orin安装Anaconda、Cuda、Cudnn、Pytorch、Tensorrt最全教程_anaconda jetson_江南綿雨的博客-CSDN博客](https://blog.csdn.net/weixin_43702653/article/details/129249585?spm=1001.2014.3001.5506)]给盒子安装环境。安装完成后就有anaconda,cuda,cudnn,pytorch,torchvision。

这一步遇到的bug ***E: Unable to [locate](https://so.csdn.net/so/search?q=locate&spm=1001.2101.3001.7020) package nvidia-jetpack***，参考这个[博客][[(22条消息) 【Jetson Agx Orin】执行sudo apt install nvidia-jetpack命令时报错：E: Unable to locate package nvidia-jetpack_Black__Jacket的博客-CSDN博客](https://blog.csdn.net/Black__Jacket/article/details/127736938?spm=1001.2014.3001.5506)]解决。

6.装好基本环境之后就是安装追踪器所需要的环境,编写调用飞机摄像头读取图像并进行处理的python代码

7.运行中遇到的bug，通过第五步系统自动装的opencv存在问题，每次一调用cv.imshow程序就无响应。解决方法是参考这个[博客][[(22条消息) Jetson带CUDA编译的opencv4.5安装教程与踩坑指南，cmake配置很重要！_jetson opencv4.5.4contrib安装_吾系桉宁的博客-CSDN博客](https://blog.csdn.net/weixin_39298885/article/details/110851373?spm=1001.2014.3001.5506)]卸载原有的opencv重新编译opencv即可。



代码里的src/tracking.py就是编写的调取摄像头，读取图像的代码。subscribe_test.py是写的订阅tracker发布的跟踪目标坐标框的信息的示例程序。





