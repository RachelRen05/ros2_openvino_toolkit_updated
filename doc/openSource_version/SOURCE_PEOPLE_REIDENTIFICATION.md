# People Reidentification
## Demo Result Snapshots
See below pictures for the demo result snapshots.
* Person Reidentification input from standard camera
![person_reidentification_demo_video](https://github.com/intel/ros2_openvino_toolkit/blob/devel/data/images/person-reidentification.gif "person reidentification demo video")
## Launching
### OpenSource Version
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
### Binary Version
* download the optimized Intermediate Representation (IR) of model (excute once)
	```bash
  cd /opt/intel/openvino/deployment_tools/tools/model_downloader
	sudo python3 downloader.py --name person-detection-retail-0013
	sudo python3 downloader.py --name person-reidentification-retail-0076
	```
 * run person reidentification sample code input from StandardCamera.
	```bash
	ros2 launch dynamic_vino_sample pipeline_reidentification.launch.py
  ```
