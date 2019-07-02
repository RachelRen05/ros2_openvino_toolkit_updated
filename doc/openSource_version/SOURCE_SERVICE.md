# Service
* run object detection service sample code input from Image  
  Run image processing service:
	```bash
	ros2 launch dynamic_vino_sample image_object_server_oss.launch.py
	```
  Run example application with an absolute path of an image on another console:
	```bash
	ros2 run dynamic_vino_sample image_object_client ~/Pictures/car.png
	```
* run face detection service sample code input from Image  
  Run image processing service:
	```bash
	ros2 launch dynamic_vino_sample image_people_server_oss.launch.py
	```
  Run example application with an absolute path of an image on another console:
	```bash
	ros2 run dynamic_vino_sample image_people_client ~/Pictures/face.png
	```
