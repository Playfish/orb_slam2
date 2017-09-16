# orb_slam2

This Project just contains ros node, inheritance with [ORB_SLAM2](https://github.com/raulmur/ORB_SLAM2), and this project goal is to simplify the steps for who want to use orb slam with ROS.

## Preparation

### Pangolin
You should install dependency library named ``[Pangolin](https://github.com/stevenlovegrove/Pangolin)``, almost steups you can use following command:

```
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
mkdir build
cd build
cmake -DCPP11_NO_BOOST=1 ..
make -j
sudo make install
```

### OpenCV3

#### Checkout
In this case you should make sure your cv_bridge is link opencv3 in ROS, you can check file conten with under command:
```
sudo vi /opt/ros/indigo/share/cv_bridge/cmake/cv_bridgeConfig.cmake
```

If in your cmake file has include following content, do [install cv_bridge](http://github.com/Playfish/orb_slam2#Install%20cv_bridge).

```
set(libraries "cv_bridge;/usr/lib/x86_64-linux-gnu/libopencv_videostab.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_video.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_superres.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_stitching.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_photo.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_ocl.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_objdetect.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_ml.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_legacy.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_imgproc.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_highgui.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_gpu.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_flann.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_features2d.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_core.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_contrib.so.2.4.8;/usr/lib/x86_64-linux-gnu/libopencv_calib3d.so.2.4.8")
```

#### Install cv_bridge

Before begin this chapter make sure you have install [OpenCV3.0](http://opencv.org/opencv-3-0.html) and above, if you have not install, typing under command:
```
wget https://github.com/opencv/opencv/archive/3.0.0.zip
unzip 3.0.0.zip
cd opencv-3.0.0
mkdir build 
cd build
cmake ..
make -j
sudo make install
```

Checkout if OpenCV install or not enter:
```
pkg-config --modversion opencv
```

If successfully will show under:
```
3.0.0
```

And now we recomplie [cv_bridge](https://github.com/ros-perception/vision_opencv) source code:
```
cd <your_catkin_ws>/src
git clone https://github.com/ros-perception/vision_opencv
cd ..
catkin_make
```

## Installation

Download code and complie with ```catkin_make```:
```
cd <your_catkin_ws>/src
git clone http://github.com/Playfish/orb_slam2
cd ..
catkin_make
```

### Usage

If you want use orb slam2 with Roch following under lines:

#### Driver With Roch
```
roslaunch roch_bringup minimal.launch
```

#### Driver with Sensors
Notes: Because will use 3d sensor for orb so donnot set ROCH_3D_SENSOR_ENABLE to false.
```
roslaunch roch_bringup sensor.launch
```

#### ORB_SLAM2

```
roscd orb_slam2/Vocabulary
tar -xvf ORBvoc.txt.tar.gz # this line just once
roslaunch orb_slam2 rgbd.launch
```

