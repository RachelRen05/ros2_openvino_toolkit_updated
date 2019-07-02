# Face Detection
* download the optimized Intermediate Representation (IR) of model (excute _once_)<br>
```bash
cd /opt/openvino_toolkit/open_model_zoo/model_downloader
python3 downloader.py --name face-detection-adas-0001
python3 downloader.py --name age-gender-recognition-retail-0013
python3 downloader.py --name emotions-recognition-retail-0003
python3 downloader.py --name head-pose-estimation-adas-0001
```
* copy label files (excute _once_)<br>
```bash
sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/emotions-recognition/FP32/emotions-recognition-retail-0003.labels /opt/openvino_toolkit/open_model_zoo/model_downloader/Retail/object_attributes/emotions_recognition/0003/dldt
sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/face_detection/face-detection-adas-0001.labels /opt/openvino_toolkit/open_model_zoo/model_downloader/Transportation/object_detection/face/pruned_mobilenet_reduced_ssd_shared_weights/dldt
```
* run face detection sample code input from StandardCamera.
	```bash
	ros2 launch dynamic_vino_sample pipeline_people_oss.launch.py
	```
* run face detection sample code input from Image.
	```bash
	ros2 launch dynamic_vino_sample pipeline_image_oss.launch.py
	```
