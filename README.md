# Custom-live-image-training-in-jetson-nano-developer-kit
* At first, do the steps in https://github.com/nithin-aikkattumadathil/Live-Image-processing-on-Jetson-Nano-Developer-Kit
* The above step will fulfill the dependencies for training your own custom model

# Training your own custom models
* cd jetson-inference
* docker/run.sh
* The above step will allow you to enter into the root of jetson nano developer kit
* v4l2-ctl --list-devices
* The above step will check whether v4l2 device, ie,camera is connected or not, if connected it will show /dev/video0 or /dev/video1
* camera-capture /dev/video0
* The above step will start the camera capture tool for training
* In the data capture control, change dataset type from classification to detection
  
