# People Reidentification
* download the optimized Intermediate Representation (IR) of model (excute _once_)<br>
```bash
cd /opt/openvino_toolkit/open_model_zoo/model_downloader
python3 downloader.py --name person-detection-retail-0013
python3 downloader.py --name person-reidentification-retail-0076
```
* run person reidentification sample code input from StandardCamera.
```bash
ros2 launch dynamic_vino_sample pipeline_reidentification_oss.launch.py
```
