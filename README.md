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
  
  * [face detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_FACE_DETECTION.md)
  * [object detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_OBJECT_DETECTION.md)
  * [object segmentation](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_OBJECT_SEGMENTATION.md)
  * [person reidentification](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_PEOPLE_REIDENTIFICATION.md)
  * [vehicle detection](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_VEHICLE_DETECTION.md)
  * [service](https://github.com/RachelRen05/ros2_openvino_toolkit_updated/blob/master/doc/openSource_version/SOURCE_SERVICE.md)
