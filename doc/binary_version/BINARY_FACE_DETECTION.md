# Face Detection
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
* run face detection sample code input from StandardCamera.(connect Intel® Neural Compute Stick 2)
	```bash
	ros2 launch dynamic_vino_sample pipeline_people_myriad.launch.py
	```
* run face detection sample code input from Image.
	```bash
	ros2 launch dynamic_vino_sample pipeline_image.launch.py
	```
