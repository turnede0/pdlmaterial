# YOLOv3 Training 

This directory will explain how to train a YoloV3 and Yolov3-Tiny model.

I have included a zip file containing all of the notebooks required to train both Yolov3 and Yolov3 Tiny

<figure>
  <a href="Yolo_Notebooks.zip" download>Download ZIP</a>
</figure>

Run the following blocks of code in Google Colab. Only some blocks will be explained

Before starting create a new folder on your drive called yolov3 and insert the zip file of your dataset there

```python
#Check if NVIDIA GPU is enabled
!nvidia-smi
```
### Mounts Google Drive folder onto colab so that you can access files on your drive.

```python
from google.colab import drive
drive.mount('/content/gdrive')
!ln -s /content/gdrive/My\ Drive/ /mydrive
!ls /mydrive
```

### Gets all the required files for Darknet

```python
# Clone
!git clone https://github.com/AlexeyAB/darknet
```
### Sets up the Darknet config for enabling GPU and Cuda

```python
# Configure
%cd darknet
!sed -i 's/OPENCV=0/OPENCV=1/' Makefile
!sed -i 's/GPU=0/GPU=1/' Makefile
!sed -i 's/CUDNN=0/CUDNN=1/' Makefile
```

### Compiles the Darknet config so that you are able to train

```python
# Compile
!make
```

### Makes a copy of yolov3.cfg and names it yolov3_training.cfg

```python
# Make a copy of yolov3.cfg
!cp cfg/yolov3.cfg cfg/yolov3_training.cfg
```

### For Yolov3_tiny replace the above with

```python
# Make a copy of yolov3.cfg
!cp cfg/yolov3-tiny.cfg cfg/yolov3_new_tiny.cfg
```

### We will have to modify some of the lines in the CFG such that the model can be trained for our custom dataset.

There are a few things to change for standard Yolov3. 

The number of classes has to be modified to be equal to the number of classes in your dataset it cannot be less.

The number of filters in the linear layer. This can be calculated by (n+5)x3 where n is the number of classes.

The batch size which can be kept at 64 but is changed based on your GPU.

The subdivision can be left at 16. 

The max_batches which is the number of iterations to train your model. Set minimum to 4000. At least 2000 per class if possible for better results.

Some augmentation in this example angle. You can open the cfg and check the top section for more options.

```python
# Change lines in yolov3_training.cfg file
# please make sure the name of your cfg file matches the below
!sed -i 's/batch=1/batch=64/' cfg/yolov3_training.cfg # Make sure they have the same cfg name
!sed -i 's/subdivisions=1/subdivisions=16/' cfg/yolov3_training.cfg
!sed -i 's/max_batches = 500200/max_batches = 6000/' cfg/yolov3_training.cfg
!sed -i 's/angle = 0/angle = 45/' cfg/yolov3_training.cfg
!sed -i '610 s@classes=80@classes=2@' cfg/yolov3_training.cfg
!sed -i '696 s@classes=80@classes=2@' cfg/yolov3_training.cfg
!sed -i '783 s@classes=80@classes=2@' cfg/yolov3_training.cfg
!sed -i '603 s@filters=255@filters=21@' cfg/yolov3_training.cfg
!sed -i '689 s@filters=255@filters=21@' cfg/yolov3_training.cfg
!sed -i '776 s@filters=255@filters=21@' cfg/yolov3_training.cfg
```

### For Yolov3_tiny modify the following lines instead. The same rules apply to Yolov3 and Yolov3 tiny

```python
# Change lines in yolov3_new_tiny.cfg file
# Please make sure the name of your cfg file matches the below
!sed -i 's/batch=1/batch=64/' cfg/yolov3_new_tiny.cfg # Make sure they have the same cfg name
!sed -i 's/subdivisions=1/subdivisions=16/' cfg/yolov3_new_tiny.cfg
!sed -i 's/max_batches = 500200/max_batches = 6000/' cfg/yolov3_new_tiny.cfg
!sed -i '135 s@classes=80@classes=2@' cfg/yolov3_new_tiny.cfg
!sed -i '177 s@classes=80@classes=2@' cfg/yolov3_new_tiny.cfg
!sed -i '127 s@filters=255@filters=21@' cfg/yolov3_new_tiny.cfg
!sed -i '171 s@filters=255@filters=21@' cfg/yolov3_new_tiny.cfg
```

### This function helps give the classes names please update PCB\nCopper to whatever your class names are

```python
!echo -e 'PCB\nCopper' > data/obj.names
!echo -e 'classes= 2\ntrain  = data/train.txt\nvalid  = data/test.txt\nnames = data/obj.names\nbackup = /mydrive/yolov3' > data/obj.data
```

### This function saves a cfg file and classes.txt to drive for use to run the model after training

```python
!cp cfg/yolov3_training.cfg /mydrive/yolov3/yolov3_testing.cfg # make sure your cfg file name matched the one you made earlier
!cp data/obj.names /mydrive/yolov3/classes.txt
```

### This block helps unzip the dataset from drive to the darknet folder.

### Make sure to replace PCBD.zip with your dataset

```python
!mkdir data/obj
!unzip /mydrive/yolov3/PCBD.zip -d data/obj
```

### This block helps create the train.txt file required to pass the dataset when training. 

### Please update PCBD with your dataset name

```python
import glob
images_list = glob.glob("data/obj/PCBD/*.jpg") # todo update PCBD with your dataset name
with open("data/train.txt", "w") as f:
    f.write("\n".join(images_list))
```
### This block helps download the weights for yolov3 used to do transfer learning

```python
!wget https://pjreddie.com/media/files/darknet53.conv.74
```
### For Yolov3-tiny please use this block instead

```python
!wget https://pjreddie.com/media/files/yolov3-tiny.weights
!./darknet partial cfg/yolov3-tiny.cfg yolov3-tiny.weights yolov3-tiny.conv.15 
```
### To start training run the following block make sure to replace your cfg name with the correct name and the darknet53.conv.74 with yolov3-tiny.conv.15  

```python
!./darknet detector train data/obj.data cfg/yolov3_training.cfg darknet53.conv.74 -dont_show
# Uncomment below and comment above to re-start your training from last saved weights
#!./darknet detector train data/obj.data cfg/yolov3_training.cfg /mydrive/yolov3/yolov3_training_last.weights -dont_show
``` 