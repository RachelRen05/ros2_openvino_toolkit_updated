# Introduction

The OpenVINO™ (Open visual inference and neural network optimization) toolkit provides a ROS-adaptered runtime framework of neural network which quickly deploys applications and solutions for vision inference. By leveraging Intel® OpenVINO™ toolkit and corresponding libraries, this runtime framework extends  workloads across Intel® hardware (including accelerators) and maximizes performance.
* Enables CNN-based deep learning inference at the edge
* Supports heterogeneous execution across computer vision accelerators—CPU, GPU, Intel® Movidius™ Neural Compute Stick, and FPGA—using a common API
* Speeds up time to market via a library of functions and preoptimized kernels
* Includes optimized calls for OpenCV and OpenVX*

## Design Architecture
From the view of hirarchical architecture design, the package is divided into different functional components, as shown in below picture. 

![OpenVINO_Architecture](https://github.com/intel/ros2_openvino_toolkit/blob/devel/data/images/design_arch.PNG "OpenVINO RunTime Architecture")

- **Intel® OpenVINO™ toolkit** is leveraged to provide deep learning basic implementation for data inference. is free software that helps developers and data scientists speed up computer vision workloads, streamline deep learning inference and deployments,
and enable easy, heterogeneous execution across Intel® platforms from edge to cloud. It helps to:
   - Increase deep learning workload performance up to 19x1 with computer vision accelerators from Intel.
   - Unleash convolutional neural network (CNN)-based deep learning inference using a common API.
   - Speed development using optimized OpenCV* and OpenVX* functions.
- **ROS2 OpenVINO Runtime Framework** is the main body of this repo. it provides key logic implementation for pipeline lifecycle management, resource management and ROS system adapter, which extends Intel OpenVINO toolkit and libraries. Furthermore, this runtime framework provides ways to ease launching, configuration and data analytics and re-use.
- **Diversal Input resources** are the data resources to be infered and analyzed with the OpenVINO framework.
- **ROS interfaces and outputs** currently include _Topic_ and _service_. Natively, RViz output and CV image window output are also supported by refactoring topic message and inferrence results.
- **Optimized Models** provides by Model Optimizer component of Intel® OpenVINO™ toolkit. Imports trained models from various frameworks (Caffe*, Tensorflow*, MxNet*, ONNX*, Kaldi*) and converts them to a unified intermediate representation file. It also optimizes topologies through node merging, horizontal fusion, eliminating batch normalization, and quantization.It also supports graph freeze and graph summarize along with dynamic input freezing.

## Logic Flow
From the view of logic implementation, the package introduces the definitions of parameter manager, pipeline and pipeline manager. The below picture depicts how these entities co-work together when the corresponding program is launched.

![Logic_Flow](https://github.com/intel/ros2_openvino_toolkit/blob/devel/data/images/impletation_logic.PNG "OpenVINO RunTime Logic Flow")

Once a corresponding program is launched with a specified .yaml config file passed in the .launch.py file or via commandline, _**parameter manager**_ analyzes the configurations about pipeline and the whole framework, then shares the parsed configuration information with pipeline procedure. A _**pipeline instance**_ is created by following the configuration info and is added into _**pipeline manager**_ for lifecycle control and inference action triggering.

The contents in **.yaml config file** should be well structured and follow the supported rules and entity names. Please see [the configuration guidance](https://github.com/intel/ros2_openvino_toolkit/blob/devel/doc/YAML_CONFIGURATION_GUIDE.md) for how to create or edit the config files.

**Pipeline** fulfills the whole data handling process: initiliazing Input Component for image data gathering and formating; building up the structured inference network and passing the formatted data through the inference network; transfering the inference results and handling output, etc.

**Pipeline manager** manages all the created pipelines according to the inference requests or external demands (say, system exception, resource limitation, or end user's operation). Because of co-working with resource management and being aware of the whole framework, it covers the ability of performance optimization by sharing system resource between pipelines and reducing the burden of data copy.

# Supported Features
## Diversal Input Components
Currently, the package support several kinds of input resources of gaining image data:

|Input Resource|Description|
|--------------------|------------------------------------------------------------------|
|StandardCamera|Any RGB camera with USB port supporting. Currently only the first USB camera if many are connected.|
|RealSenseCamera| Intel RealSense RGB-D Camera, directly calling RealSense Camera via librealsense plugin of openCV.|
|Image Topic| any ROS topic which is structured in image message.|
|Image File| Any image file which can be parsed by openCV, such as .png, .jpeg.|
|Video File| Any video file which can be parsed by openCV.|

## Inference Implementations
Currently, the inference feature list is supported:

|Inference|Description|
|-----------------------|------------------------------------------------------------------|
|Face Detection|Object Detection task applied to face recognition using a sequence of neural networks.|
|Emotion Recognition| Emotion recognition based on detected face image.|
|Age & Gender Recognition| Age and gener recognition based on detected face image.|
|Head Pose Estimation| Head pose estimation based on detected face image.|
|Object Detection| object detection based on SSD-based trained models.|
|Vehicle Detection| Vehicle and passenger detection based on Intel models.|
|Object Segmentation| object detection and segmentation.|
|Person Reidentification| Person Reidentification based on object detection.|

## ROS interfaces and outputs
### Topic
#### Subscribed Topic
- Image topic:
```/openvino_toolkit/image_raw```([sensor_msgs::msg::Image](https://github.com/ros2/common_interfaces/blob/master/sensor_msgs/msg/Image.msg))
#### Published Topic
- Face Detection:
```/ros2_openvino_toolkit/face_detection```([object_msgs:msg:ObjectsInBoxes](https://github.com/intel/ros2_object_msgs/blob/master/msg/ObjectsInBoxes.msg))
- Emotion Recognition:
```/ros2_openvino_toolkit/emotions_recognition```([people_msgs:msg:EmotionsStamped](https://github.com/intel/ros2_openvino_toolkit/blob/master/people_msgs/msg/EmotionsStamped.msg))
- Age and Gender Recognition:
```/ros2_openvino_toolkit/age_genders_Recognition```([people_msgs:msg:AgeGenderStamped](https://github.com/intel/ros2_openvino_toolkit/blob/master/people_msgs/msg/AgeGenderStamped.msg))
- Head Pose Estimation:
```/ros2_openvino_toolkit/headposes_estimation```([people_msgs:msg:HeadPoseStamped](https://github.com/intel/ros2_openvino_toolkit/blob/master/people_msgs/msg/HeadPoseStamped.msg))
- Object Detection:
```/ros2_openvino_toolkit/detected_objects```([object_msgs::msg::ObjectsInBoxes](https://github.com/intel/ros2_object_msgs/blob/master/msg/ObjectsInBoxes.msg))
- Object Segmentation:
```/ros2_openvino_toolkit/segmented_obejcts```([people_msgs::msg::ObjectsInMasks](https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/msg/ObjectsInMasks.msg))
- Person Reidentification:
```/ros2_openvino_toolkit/reidentified_persons```([people_msgs::msg::ReidentificationStamped](https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/msg/ReidentificationStamped.msg))
- Vehicle Detection:
```/ros2_openvino_toolkit/detected_license_plates```([people_msgs::msg::VehicleAttribsStamped]https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/msg/VehicleAttribsStamped.msg)
- Vehicle License Detection:
```/ros2_openvino_toolkit/detected_license_plates```([people_msgs::msg::LicensePlateStamped]https://github.com/intel/ros2_openvino_toolkit/blob/devel/people_msgs/msg/LicensePlateStamped.msg)
- Rviz Output:
```/ros2_openvino_toolkit/image_rviz```([sensor_msgs::msg::Image](https://github.com/ros2/common_interfaces/blob/master/sensor_msgs/msg/Image.msg))

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

## Demo Result Snapshots
See below pictures for the demo result snapshots.
* face detection input from image
![face_detection_demo_image](https://github.com/intel/ros2_openvino_toolkit/blob/devel/data/images/face_detection.png "face detection demo image")

* object detection input from realsense camera
![object_detection_demo_realsense](https://github.com/intel/ros2_openvino_toolkit/blob/devel/data/images/object_detection.gif "object detection demo realsense")

* object segmentation input from video
![object_segmentation_demo_video](https://github.com/intel/ros2_openvino_toolkit/blob/devel/data/images/object_segmentation.gif "object segmentation demo video")

* Person Reidentification input from standard camera
![person_reidentification_demo_video](https://github.com/intel/ros2_openvino_toolkit/blob/devel/data/images/person-reidentification.gif "person reidentification demo video")

# Installation & Launching
**NOTE:** Intel releases 2 different series of OpenVINO Toolkit, we call them as [OpenSource Version](https://github.com/opencv/dldt/) and [Tarball Version](https://software.intel.com/en-us/openvino-toolkit). 

## Dependencies Installation
### OpenSource Version
One-step installation scripts are provided for the dependencies' installation. Please see [the guide](https://github.com/intel/ros2_openvino_toolkit/blob/devel/doc/OPEN_SOURCE_CODE_README.md) for details.

### Tarball Version
One-step installation scripts are provided for the dependencies' installation. Please see [the guide](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/binary_version/BINARY_INSTALLATION.md) for details.

## Launching
### Preparation
* Configure the Neural Compute Stick USB Driver
```bash
cd ~/Downloads
SUBSYSTEM=="usb", ATTRS{idProduct}=="2150", ATTRS{idVendor}=="03e7", GROUP="users", MODE="0666", ENV{ID_MM_DEVICE_IGNORE}="1"
SUBSYSTEM=="usb", ATTRS{idProduct}=="2485", ATTRS{idVendor}=="03e7", GROUP="users", MODE="0666", ENV{ID_MM_DEVICE_IGNORE}="1"
SUBSYSTEM=="usb", ATTRS{idProduct}=="f63b", ATTRS{idVendor}=="03e7", GROUP="users", MODE="0666", ENV{ID_MM_DEVICE_IGNORE}="1"
EOF
sudo cp 97-usbboot.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules
sudo udevadm trigger
sudo ldconfig
rm 97-usbboot.rules
```
### OpenSource Version
* set ENV LD_LIBRARY_PATH<br>
```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/openvino_toolkit/dldt/inference-engine/bin/intel64/Release/lib
```
* install prerequisites
```bash
cd /opt/openvino_toolkit/dldt/model-optimizer/install_prerequisites
sudo ./install_prerequisites.sh
```
* [face detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_FACE_DETECTION.md)
* [object detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_OBJECT_DETECTION.md)
* [object segmentation](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_OBJECT_SEGMENTATION.md)
* [person reidentification](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_PEOPLE_REIDENTIFICATION.md)
* [vehicle detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_VEHICLE_DETECTION.md)
* [service](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_SERVICE.md)

### Tarball Version
* set ENV LD_LIBRARY_PATH and environment
```bash
source /opt/intel/openvino/bin/setupvars.sh
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/intel/openvino/deployment_tools/inference_engine/samples/build/intel64/Release/lib
```
* install prerequisites
```bash
cd /opt/intel/openvino/deployment_tools/model_optimizer/install_prerequisites
sudo ./install_prerequisites.sh
```
* [face detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/binary_version/BINARY_FACE_DETECTION.md)
* [object detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/binary_version/BINARY_OBJECT_DETECTION.md)
* [object segmentation](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/binary_version/BINARY_OBJECT_SEGMENTATION.md)
* [person reidentification](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/binary_version/BINARY_PEOPLE_REIDENTIFICATION.md)
* [vehicle detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/binary_version/BINARY_VEHICLE_DETECTION.md)
* [service](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/binary_version/BINARY_SERVICE.md)

# TODO Features
* Support **result filtering** for inference process, so that the inference results can be filtered to different subsidiary inference. For example, given an image, firstly we do Object Detection on it, secondly we pass cars to vehicle brand recognition and pass license plate to license number recognition.
* Design **resource manager** to better use such resources as models, engines, and other external plugins.
* Develop GUI based **configuration and management tools** (and monitoring and diagnose tools), in order to provide easy entry for end users to simplify their operation. 

# More Information
* ROS2 OpenVINO discription writen in Chinese: https://mp.weixin.qq.com/s/BgG3RGauv5pmHzV_hkVAdw 

###### *Any security issue should be reported using process at https://01.org/security*

