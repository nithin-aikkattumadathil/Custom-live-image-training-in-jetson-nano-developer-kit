# Custom-live-image-training-in-jetson-nano-developer-kit
* At first, do the steps in https://github.com/nithin-aikkattumadathil/Live-Image-processing-on-Jetson-Nano-Developer-Kit
* The above step will fulfill the dependencies for training your own custom model

# Capturing your own custom models
* cd jetson-inference
* docker/run.sh
* The above step will allow you to enter into the root of jetson nano developer kit
* v4l2-ctl --list-devices
* The above step will check whether v4l2 device, ie,camera is connected or not, if connected it will show /dev/video0 or /dev/video1
* camera-capture /dev/video0
* The above step will start the camera capture tool for training
* In the data capture control, change dataset type from classification to detection
* at the same time, create a new folder to save your training model, and name it accordingly within jetson-inference/python/training/detection/ssd/data
* enter into the folder and create a new text file naming labels.txt, you can use ' gedit labels.txt ' command for that
* list the name of objects you are going to train in the labels.txt file, one name in one line and top left alligned,and save.
* now in the data capture control, choose the dataset path as, jetson-inference/python/training/detection/ssd/data/model_name
* now choose class label path as jetson-inference/python/training/detection/ssd/data/model_name/labels.txt
* In the data capture control, untick clear on unfreeze and tick merge sets
* place the object infront of the camera
* click the freeze button and draw the boundary box at the perimeter of object in the camera
* keep the boundary box tight to the object,and check the object class is correct in the data capture control.
* move the object around and save a minimum of 100 images of each object classes
* we can train multiple classes at the same time, for every new class , freeze camera to draw boundary
* close the camera capture tool after taking images

# Training your own custom models
* When you've collected a bunch of data, then you can try training a model on it using the same train_ssd.py script.
* Disble desktop GUI to free up extra memory that the window manager and desktop uses, using command ' sudo init 3 '
* open a new terminal in root jetson-inference/python/training/detection/ssd
* start the training using epoch using following command,it  will take some time
* python3 train_ssd.py --dataset-type=voc --data=data/model_name --model-dir=models/model_name --batch-size=2 --workers=1 --epochs=1
* after training you'll need to convert your PyTorch model to ONNX
* python3 onnx_export.py --model-dir=models/model_name
* The converted model will then be saved under model_name/ssd-mobilenet.onnx, which you can then load with the detectnet programs
* make sure you are in root jetson-inference/python/training/detection/ssd
* detectnet --model=models/model_name/ssd-mobilenet.onnx --labels=models/model_name/labels.txt --input-blob-input_0 --output-cvg=scores --output-bbox=boxes /dev/video0
* the above command will start the detection procedures

# That's all ! You can train your jetson to detect anything according to your project
  
