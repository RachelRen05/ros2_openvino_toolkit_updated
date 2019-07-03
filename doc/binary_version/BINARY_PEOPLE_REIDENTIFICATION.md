# Person Reidentification
* download the optimized Intermediate Representation (IR) of model (excute once)
	```bash
	sudo python3 downloader.py --name person-detection-retail-0013
	sudo python3 downloader.py --name person-reidentification-retail-0076
	```
 * run person reidentification sample code input from StandardCamera.
	```bash
	ros2 launch dynamic_vino_sample pipeline_reidentification.launch.py
