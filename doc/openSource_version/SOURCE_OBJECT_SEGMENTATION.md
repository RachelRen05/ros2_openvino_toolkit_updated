# Object Segmentation
## Demo Result Snapshots
See below pictures for the demo result snapshots.
* object segmentation input from video
![object_segmentation_demo_video](https://github.com/intel/ros2_openvino_toolkit/blob/devel/data/images/object_segmentation.gif "object segmentation demo video")
## Launching
### OpenSource Version
* download and convert a trained model to produce an optimized Intermediate Representation (IR) of the model 
	```bash
	#object segmentation model
	cd /opt/openvino_toolkit/dldt/model-optimizer/install_prerequisites
	sudo ./install_prerequisites.sh
	mkdir -p ~/Downloads/models
	cd ~/Downloads/models
	wget http://download.tensorflow.org/models/object_detection/mask_rcnn_inception_v2_coco_2018_01_28.tar.gz
	tar -zxvf mask_rcnn_inception_v2_coco_2018_01_28.tar.gz
	cd mask_rcnn_inception_v2_coco_2018_01_28
	python3 /opt/openvino_toolkit/dldt/model-optimizer/mo_tf.py --input_model frozen_inference_graph.pb --tensorflow_use_custom_operations_config /opt/openvino_toolkit/dldt/model-optimizer/extensions/front/tf/mask_rcnn_support.json --tensorflow_object_detection_api_pipeline_config pipeline.config --reverse_input_channels --output_dir ./output/
	sudo mkdir -p /opt/models
	sudo ln -s ~/Downloads/models/mask_rcnn_inception_v2_coco_2018_01_28 /opt/models/
	```
* copy label files (excute _once_)<br>
	```bash
	sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/object_segmentation/frozen_inference_graph.labels /opt/models/mask_rcnn_inception_v2_coco_2018_01_28/output
	```
* run object segmentation sample code input from RealSenseCameraTopic.
	```bash
	ros2 launch dynamic_vino_sample pipeline_segmentation.launch.py
	```
* run object segmentation sample code input from Video.
	```bash
	ros2 launch dynamic_vino_sample pipeline_video.launch.py
	```
### Binary Version
* download and convert a trained model to produce an optimized Intermediate Representation (IR) of the model 
	```bash
	#object segmentation model
	mkdir -p ~/Downloads/models
	cd ~/Downloads/models
	wget http://download.tensorflow.org/models/object_detection/mask_rcnn_inception_v2_coco_2018_01_28.tar.gz
	tar -zxvf mask_rcnn_inception_v2_coco_2018_01_28.tar.gz
	cd mask_rcnn_inception_v2_coco_2018_01_28
	python3 /opt/intel/openvino/deployment_tools/model_optimizer/mo_tf.py --input_model frozen_inference_graph.pb --tensorflow_use_custom_operations_config /opt/intel/openvino/deployment_tools/model_optimizer/extensions/front/tf/mask_rcnn_support.json --tensorflow_object_detection_api_pipeline_config pipeline.config --reverse_input_channels --output_dir ./output/
	sudo mkdir -p /opt/models
	sudo ln -sf ~/Downloads/models/mask_rcnn_inception_v2_coco_2018_01_28 /opt/models/
	```
* copy label files (excute _once_)<br>
	```bash
	sudo cp /opt/openvino_toolkit/ros2_openvino_toolkit/data/labels/object_segmentation/frozen_inference_graph.labels ~/Downloads/models/mask_rcnn_inception_v2_coco_2018_01_28/output
	```
* run object segmentation sample code input from RealSenseCameraTopic.(connect Intel® Neural Compute Stick 2)
	```bash
	ros2 launch dynamic_vino_sample pipeline_segmentation.launch.py
	```
* run object segmentation sample code input from Video.
	```bash
	ros2 launch dynamic_vino_sample pipeline_video.launch.py
	```
