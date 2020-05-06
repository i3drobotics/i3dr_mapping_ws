# Setup
Initalise wstool
```
cd src
wstool init
```

Add Kimera packages to wstool
```
wstool merge Kimera-VIO-ROS/install/kimera_vio_ros_https.rosinstall
```

Add i3dr packages to wstool
```
wstool merge i3dr_stereo_camera-ros/install/i3dr_stereo_camera_ros_https.rosinstall
```

Update wstool (download packages)
```
wstool update
```

# Build
Build workspace
```
catkin init
catkin config --cmake-args -DCMAKE_BUILD_TYPE=Release -DGTSAM_USE_SYSTEM_EIGEN=ON -DWITH_CUDA=ON -DWITH_I3DR_ALG=ON -DWITH_QT=ON
catkin build
```

*NOTE: This might take a while as it builds opencv from source (>1 hour on a good PC)*

Currently unable to build tests when using master branch of gscam so if you need to build tests then switch to the develop branch, otherwise use build option -DBUILD_TESTS=OFF.
See issue [here](https://github.com/MIT-SPARK/Kimera-VIO-ROS/issues/53)
```
cd PATH_TO_REPO/src/gscam/
git checkout develop
```

# Run
## RealSense D435i

Start ros core
```
roscore
```

Start camera capture
```
roslaunch realsense2_camera rs_camera.launch unite_imu_method:=linear_interpolation
```

Disable infrared projector (affects tracking)
```
rosrun dynamic_reconfigure dynparam set /camera/stereo_module emitter_enabled 0
```

Start tracking
```
roslaunch kimera_vio_ros kimera_vio_ros_i3dr_deimos.launch
```

View in rviz
```
rviz -d $(rospack find kimera_vio_ros)/rviz/kimera_vio_realsense.rviz
```

## Run with I3DR Deimos

Start ros core
```
roscore
```

Start camera capture
```
roslaunch i3dr_deimos deimos.launch 
```

Disable infrared projector (affects tracking)
```
Turn off switch at the back of the Deimos camera
```

Start tracking
```
roslaunch kimera_vio_ros kimera_vio_ros_realsense_IR.launch
```

View in rviz
```
rviz -d $(rospack find kimera_vio_ros)/rviz/kimera_vio_realsense.rviz
```