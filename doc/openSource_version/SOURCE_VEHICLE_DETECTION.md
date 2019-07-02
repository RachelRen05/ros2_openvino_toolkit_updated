# Vehicle Detection
* download the optimized Intermediate Representation (IR) of model (excute _once_)<br>
```bash
cd /opt/openvino_toolkit/open_model_zoo/model_downloader
python3 downloader.py --name vehicle-license-plate-detection-barrier-0106
python3 downloader.py --name vehicle-attributes-recognition-barrier-0039
python3 downloader.py --name license-plate-recognition-barrier-0001
```
* copy label files (excute _once_)<br>
```bash
sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/object_detection/vehicle-license-plate-detection-barrier-0106.labels /opt/openvino_toolkit/open_model_zoo/model_downloader/Security/object_detection/barrier/0106/dldt
```
* run vehicle detection sample code input from StandardCamera.
```bash
ros2 launch dynamic_vino_sample pipeline_vehicle_detection_oss.launch.py
```
