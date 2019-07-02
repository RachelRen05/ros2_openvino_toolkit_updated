# Object Detection
## mobilenet-ssd
* download and convert a trained model to produce an optimized Intermediate Representation (IR) of the model 
```bash
cd /opt/openvino_toolkit/open_model_zoo/model_downloader
python3 ./downloader.py --name mobilenet-ssd
#FP32 precision model
sudo python3 /opt/openvino_toolkit/dldt/model-optimizer/mo.py --input_model /opt/openvino_toolkit/open_model_zoo/model_downloader/object_detection/common/mobilenet-ssd/caffe/mobilenet-ssd.caffemodel --output_dir /opt/openvino_toolkit/open_model_zoo/model_downloader/object_detection/common/mobilenet-ssd/caffe/output/FP32 --mean_values [127.5,127.5,127.5] --scale_values [127.5]
#FP16 precision model
sudo python3 /opt/openvino_toolkit/dldt/model-optimizer/mo.py --input_model /opt/openvino_toolkit/open_model_zoo/model_downloader/object_detection/common/mobilenet-ssd/caffe/mobilenet-ssd.caffemodel --output_dir /opt/openvino_toolkit/open_model_zoo/model_downloader/object_detection/common/mobilenet-ssd/caffe/output/FP16 --data_type=FP16 --mean_values [127.5,127.5,127.5] --scale_values [127.5]
```
* copy label files (excute _once_)<br>
```bash
sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/object_detection/mobilenet-ssd.labels /opt/openvino_toolkit/open_model_zoo/model_downloader/object_detection/common/mobilenet-ssd/caffe/output/FP32
sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/object_detection/mobilenet-ssd.labels /opt/openvino_toolkit/open_model_zoo/model_downloader/object_detection/common/mobilenet-ssd/caffe/output/FP16
```
* run object detection sample code input from RealSenseCamera.(connect Intel® Neural Compute Stick 2)
```bash
ros2 launch dynamic_vino_sample pipeline_object_oss.launch.py
```
* run object detection sample code input from RealSenseCameraTopic.(connect Intel® Neural Compute Stick 2)
```bash
ros2 launch dynamic_vino_sample pipeline_object_oss_topic.launch.py
```
## YOLOv3
* Dump YOLOv3 TenorFlow* Model
  - Clone the repository: 
  ```bash
  mkdir -p ~/code && cd ~/code
  git clone https://github.com/mystic123/tensorflow-yolo-v3.git
  sudo ln -sf ~/code/tensorflow-yolo-v3 /opt/openvino_toolkit/
  ```
  - Download [coco.names](https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names) file from the DarkNet website OR use labels that fit your task. </br>
  ```bash
  cd ~/code/tensorflow-yolo-v3
  wget https://raw.githubusercontent.com/pjreddie/darknet/master/data/coco.names
  ```
  - Download the weights file: 
  ```bash
  cd ~/code/tensorflow-yolo-v3
  wget https://pjreddie.com/media/files/yolov3.weights
  ```
  - Run a converter: 
  ```bash
  cd ~/code/tensorflow-yolo-v3
  python3 convert_weights_pb.py --class_names coco.names --data_format NHWC --weights_file yolov3.weights
  ```
* Convert YOLOv3 TensorFlow Model to the IR
```bash
#FP32 precision model
sudo python3 /opt/openvino_toolkit/dldt/model-optimizer/mo.py --input_model ~/code/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb --tensorflow_use_custom_operations_config /opt/openvino_toolkit/dldt/model-optimizer/extensions/front/tf/yolo_v3.json --input_shape=[1,416,416,3] --data_type=FP32 --output_dir /opt/openvino_toolkit/tensorflow-yolo-v3/output/FP32   
#FP16 precision model
sudo python3 /opt/openvino_toolkit/dldt/model-optimizer/mo.py --input_model ~/code/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb --tensorflow_use_custom_operations_config /opt/openvino_toolkit/dldt/model-optimizer/extensions/front/tf/yolo_v3.json --input_shape=[1,416,416,3] --data_type=FP16 --output_dir /opt/openvino_toolkit/tensorflow-yolo-v3/output/FP16   
```
* run object detection sample code input from RealSenseCamera.(connect Intel® Neural Compute Stick 2)
```bash
ros2 launch dynamic_vino_sample pipeline_object_yolov3_oss.launch.py
```
* run object detection sample code input from RealSenseCameraTopic.(connect Intel® Neural Compute Stick 2)
```bash
ros2 launch dynamic_vino_sample pipeline_object_yolov3_oss_topic.launch.py
```
