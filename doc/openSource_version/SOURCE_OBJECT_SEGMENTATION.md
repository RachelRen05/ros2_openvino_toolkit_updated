# Object Segmentation
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
