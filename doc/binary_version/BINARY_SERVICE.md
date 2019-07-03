# Service
## Object Detection Service
* download and convert a trained model to produce an optimized Intermediate Representation (IR) of the model 
```bash
cd /opt/intel/openvino/deployment_tools/tools/model_downloader
sudo python3 ./downloader.py --name mobilenet-ssd
#FP32 precision model
sudo python3 /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model /opt/intel/openvino/deployment_tools/tools/model_downloader/object_detection/common/mobilenet-ssd/caffe/mobilenet-ssd.caffemodel --output_dir /opt/intel/openvino/deployment_tools/tools/model_downloader/object_detection/common/mobilenet-ssd/caffe/output/FP32 --mean_values [127.5,127.5,127.5] --scale_values [127.5]
#FP16 precision model
sudo python3 /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model /opt/intel/openvino/deployment_tools/tools/model_downloader/object_detection/common/mobilenet-ssd/caffe/mobilenet-ssd.caffemodel --output_dir /opt/intel/openvino/deployment_tools/tools/model_downloader/object_detection/common/mobilenet-ssd/caffe/output/FP16 --data_type=FP16 --mean_values [127.5,127.5,127.5] --scale_values [127.5]
```
* copy label files (excute _once_)<br>
```bash
sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/object_detection/mobilenet-ssd.labels /opt/intel/openvino/deployment_tools/tools/model_downloader/object_detection/common/mobilenet-ssd/caffe/output/FP32
sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/object_detection/mobilenet-ssd.labels /opt/intel/openvino/deployment_tools/tools/model_downloader/object_detection/common/mobilenet-ssd/caffe/output/FP16
```
* run object detection service sample code input from Image  
  Run image processing service:
	```bash
	ros2 launch dynamic_vino_sample image_object_server.launch.py
	```
  Run example application with an absolute path of an image on another console:
	```bash
	ros2 run dynamic_vino_sample image_object_client ~/Pictures/car.png
	```
## People Detection Service
* download the optimized Intermediate Representation (IR) of model (excute once)
	```bash
	sudo python3 downloader.py --name face-detection-adas-0001
	sudo python3 downloader.py --name face-detection-adas-0001-fp16
	sudo python3 downloader.py --name age-gender-recognition-retail-0013
	sudo python3 downloader.py --name emotions-recognition-retail-0003
	```
* copy label files (excute _once_)<br>
	```bash
	sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/emotions-recognition/FP32/emotions-recognition-retail-0003.labels /opt/intel/openvino/deployment_tools/tools/model_downloader/Retail/object_attributes/emotions_recognition/0003/dldt
	sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/face_detection/face-detection-adas-0001.labels /opt/intel/openvino/deployment_tools/tools/model_downloader/Transportation/object_detection/face/pruned_mobilenet_reduced_ssd_shared_weights/dldt
	sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/face_detection/face-detection-adas-0001-fp16.labels /opt/intel/openvino/deployment_tools/tools/model_downloader/Transportation/object_detection/face/pruned_mobilenet_reduced_ssd_shared_weights/dldt
	```
* run people detection service sample code input from Image  
  Run image processing service:
	```bash
	ros2 launch dynamic_vino_sample image_people_server.launch.py
	```
  Run example application with an absolute path of an image on another console:
	```bash
	ros2 run dynamic_vino_sample image_people_client ~/Pictures/face.png
	```
