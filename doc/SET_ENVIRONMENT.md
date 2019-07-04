# Set Environment
## OpenSource Version
* set ENV LD_LIBRARY_PATH<br>
  ```bash
  export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/openvino_toolkit/dldt/inference-engine/bin/intel64/Release/lib
  ```
* install prerequisites
  ```bash
  cd /opt/openvino_toolkit/dldt/model-optimizer/install_prerequisites
  sudo ./install_prerequisites.sh
  ```
## Binary Version
* set ENV LD_LIBRARY_PATH and environment
  ```bash
  source /opt/intel/openvino/bin/setupvars.sh
  export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/intel/openvino/deployment_tools/inference_engine/samples/build/intel64/Release/lib
  ```
* install prerequisites
  ```bash
  cd /opt/intel/openvino/deployment_tools/model_optimizer/install_prerequisites
  sudo ./install_prerequisites.sh
  ```
