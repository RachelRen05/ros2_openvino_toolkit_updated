# Introduction
The OpenVINO™ (Open visual inference and neural network optimization) toolkit provides a ROS-adaptered runtime framework of neural network which quickly deploys applications and solutions for vision inference. By leveraging Intel® OpenVINO™ toolkit and corresponding libraries, this runtime framework extends  workloads across Intel® hardware (including accelerators) and maximizes performance.
* Enables CNN-based deep learning inference at the edge
* Supports heterogeneous execution across computer vision accelerators—CPU, GPU, Intel® Movidius™ Neural Compute Stick, and FPGA—using a common API
* Speeds up time to market via a library of functions and preoptimized kernels
* Includes optimized calls for OpenCV and OpenVX*

# Design Architecture and Logic Flow[guide](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/Design_Architecture_and_logic_flow.md)

# Supported Features
## Diversal Input Components
Currently, the package support several kinds of input resources of gaining image data:

|Input Resource|Description|Subscribed Topic
|---|---|---|
|StandardCamera|Any RGB camera with USB port supporting. Currently only the first USB camera if many are connected.| |
|RealSenseCamera| Intel RealSense RGB-D Camera,directly calling RealSense Camera via librealsense plugin of openCV.| |
|Image Topic| any ROS topic which is structured in image message.|```/openvino_toolkit/image_raw```([sensor_msgs::msg::Image](https://github.com/ros2/common_interfaces/blob/master/sensor_msgs/msg/Image.msg))|
|Image File| Any image file which can be parsed by openCV, such as .png, .jpeg.|
|Video File| Any video file which can be parsed by openCV.|

## Inference Implementations
Currently, the inference feature list is supported:

|Inference|Description|Outputs Topic|
|---|---|---|
|Face Detection|Object Detection task applied to face recognition using a sequence of neural networks.|```/ros2_openvino_toolkit/face_detection```([object_msgs:msg:ObjectsInBoxes](https://github.com/intel/ros2_object_msgs/blob/master/msg/ObjectsInBoxes.msg))|
|Emotion Recognition| Emotion recognition based on detected face image.|```/ros2_openvino_toolkit/emotions_recognition```([people_msgs:msg:EmotionsStamped](https://github.com/intel/ros2_openvino_toolkit/blob/master/people_msgs/msg/EmotionsStamped.msg))|
|Age & Gender Recognition| Age and gener recognition based on detected face image.|```/ros2_openvino_toolkit/age_genders_Recognition```([people_msgs:msg:AgeGenderStamped](https://github.com/intel/ros2_openvino_toolkit/blob/master/people_msgs/msg/AgeGenderStamped.msg))|
|Head Pose Estimation| Head pose estimation based on detected face image.|```/ros2_openvino_toolkit/headposes_estimation```([people_msgs:msg:HeadPoseStamped](https://github.com/intel/ros2_openvino_toolkit/blob/master/people_msgs/msg/HeadPoseStamped.msg))|
|Object Detection| object detection based on SSD-based trained models.|```/ros2_openvino_toolkit/detected_objects```([object_msgs::msg::ObjectsInBoxes](https://github.com/intel/ros2_object_msgs/blob/master/msg/ObjectsInBoxes.msg))|
|Vehicle Detection| Vehicle and license detection based on Intel models.|```/ros2_openvino_toolkit/detected_vehicles_attribs```([people_msgs::msg::VehicleAttribsStamped](https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/msg/VehicleAttribsStamped.msg))```/ros2_openvino_toolkit/detected_license_plates```([people_msgs::msg::LicensePlateStamped](https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/msg/LicensePlateStamped.msg))|
|Object Segmentation| object detection and segmentation.|```/ros2_openvino_toolkit/segmented_obejcts```([people_msgs::msg::ObjectsInMasks](https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/msg/ObjectsInMasks.msg))|
|Person Reidentification| Person Reidentification based on object detection.|```/ros2_openvino_toolkit/reidentified_persons```([people_msgs::msg::ReidentificationStamped](https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/msg/ReidentificationStamped.msg))|

## ROS interfaces and outputs
**Note**:In addition to topic output interface,there are three other output interfaces.
### Service
- Object Detection Service:
```/detect_object``` ([object_msgs::srv::DetectObject](https://github.com/intel/ros2_object_msgs/blob/master/srv/DetectObject.srv))
- Face Detection Service:
```/detect_face``` ([object_msgs::srv::DetectObject](https://github.com/intel/ros2_object_msgs/blob/master/srv/DetectObject.srv))
- Age & Gender Detection Service:
```/detect_age_gender``` ([people_msgs::srv::AgeGender](https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/srv/AgeGender.srv))
- Headpose Detection Service:
```/detect_head_pose``` ([people_msgs::srv::HeadPose](https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/srv/HeadPose.srv))
- Emotion Detection Service:
```/detect_emotion``` ([people_msgs::srv::Emotion](https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/srv/Emotion.srv))

### RViz
RViz dispaly is also supported by the composited topic of original image frame with inference result.
To show in RViz tool, add an image marker with the composited topic:
```/ros2_openvino_toolkit/image_rviz```([sensor_msgs::msg::Image](https://github.com/ros2/common_interfaces/blob/master/sensor_msgs/msg/Image.msg))

### Image Window
OpenCV based image window is natively supported by the package.
To enable window, Image Window output should be added into the output choices in .yaml config file. see [the config file guidance](https://github.com/intel/ros2_openvino_toolkit/blob/devel/doc/YAML_CONFIGURATION_GUIDE.md) for checking/adding this feature in your launching.

# Installation & Launching
**NOTE:** Intel releases 2 different series of OpenVINO Toolkit, we call them as [OpenSource Version](https://github.com/opencv/dldt/) and [Binary Version](https://software.intel.com/en-us/openvino-toolkit). 

## Dependencies Installation
### OpenSource Version
One-step installation scripts are provided for the dependencies' installation. Please see [the guide](https://github.com/intel/ros2_openvino_toolkit/blob/devel/doc/OPEN_SOURCE_CODE_README.md) for details.

### Binary Version
One-step installation scripts are provided for the dependencies' installation. Please see [the guide](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/binary_version/BINARY_INSTALLATION.md) for details.

## Launching
### set environment[guide](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/SET_ENVIRONMENT.md)
### Preparation
* Configure the Neural Compute Stick USB Driver
   ```bash
   cd ~/Downloads
   SUBSYSTEM=="usb", ATTRS{idProduct}=="2150", ATTRS{idVendor}=="03e7", GROUP="users", MODE="0666",   ENV{ID_MM_DEVICE_IGNORE}="1"
   SUBSYSTEM=="usb", ATTRS{idProduct}=="2485", ATTRS{idVendor}=="03e7", GROUP="users", MODE="0666", ENV{ID_MM_DEVICE_IGNORE}="1"
   SUBSYSTEM=="usb", ATTRS{idProduct}=="f63b", ATTRS{idVendor}=="03e7", GROUP="users", MODE="0666", ENV{ID_MM_DEVICE_IGNORE}="1"
   EOF
   sudo cp 97-usbboot.rules /etc/udev/rules.d/
   sudo udevadm control --reload-rules
   sudo udevadm trigger
   sudo ldconfig
   rm 97-usbboot.rules
   ```
* [face detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_FACE_DETECTION.md)
* [object detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_OBJECT_DETECTION.md)
* [object segmentation](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_OBJECT_SEGMENTATION.md)
* [person reidentification](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_PEOPLE_REIDENTIFICATION.md)
* [vehicle detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_VEHICLE_DETECTION.md)
* [service](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_SERVICE.md)

# TODO Features
* Support **result filtering** for inference process, so that the inference results can be filtered to different subsidiary inference. For example, given an image, firstly we do Object Detection on it, secondly we pass cars to vehicle brand recognition and pass license plate to license number recognition.
* Design **resource manager** to better use such resources as models, engines, and other external plugins.
* Develop GUI based **configuration and management tools** (and monitoring and diagnose tools), in order to provide easy entry for end users to simplify their operation. 

# More Information
* ROS2 OpenVINO discription writen in Chinese: https://mp.weixin.qq.com/s/BgG3RGauv5pmHzV_hkVAdw 

###### *Any security issue should be reported using process at https://01.org/security*

