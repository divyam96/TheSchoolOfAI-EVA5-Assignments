# Trinity Decoder
By Divyam Malay Shah (divyam096@gmail.com)
### This assignment is still incomplete. It should be complete in another 1-2 days. 

This project contains the implementation of a trinity decoder CNN that enables simultaneous depth estimation,
plane segmentation and object detection. The architecture for the same can be seen in the image below.

![Alt text](resources/TrinityArch.jpg?raw=true "Trinity Decoder")

This model uses a single pre-trained encoder from the [Intel MiDaS](https://github.com/intel-isl/MiDaS) depth estimation model.
 The output of this encoder is then fed into the following decoders:
 1. Depth Estimation: We use MiDaS decoder architecture for depth estimation. This is initialised with the weights from the intel MiDaS model.
 2. YoloV3 decoder: Here we add a few residsual blocks followed by the three YoloV3 heads for object detection.
 3. Plane Segmentation: We use an architecture inspired by the Intel MiDaS model. Instead of depth estimation we train it 
 for plane segmentation on dat obtained from the PlaneRCNN model.
 
## Data:
 
We use a custom dataset that was prepared manually and annotated using [this](https://github.com/miki998/YoloV3_Annotation_Tool)
 tool. Our dataset contains images for the following classes:
1. Hardhats
2. Vests
3. Masks
4. Boots

These classes are mentioned in [classes.txt](./classes.txt). The images and labels for our custom dataset can be downloaded from
this [drive](https://drive.google.com/file/d/1EqtOpF7cS74C56EaVQoKNkQmpT6_HFL2/view?usp=sharing) location. Please download this 
dataset and extract its contents.

Once data is downloaded and extracted, it can be loaded using the train/test split as specified by [train.txt](./train.txt) and
[test.txt](./test.txt). 

Training our model:
 
 1) Depth Estimation:
 To train our model for depth estimation we use the intel MiDaS model as a teacher model. The
  depth estimator decoder from our custom trinity model plays the role of a student. We train our depth decoder to ensure that its output
   converges to that of the MiDaS teacher model. We use the images from our custom dataset to train our depth decoder.
   
 2) YOLOV3:
  We train our YOLOV3 decoder on our custom dataset. We make use of augmentations to help our model converge faster. Our training starts on
  images of size 64. The training is to be extended on images of size 128, 254, 512.
  
  3) Plane Segmentation:
  We train our plane segmentation decoder on annotations generated by PlanerCNN for images in our custom dataset. The PlaneRCNN was modified to generate
   class annotations as per the 8 predefined [anchor planes](https://github.com/NVlabs/planercnn/blob/master/anchors/anchor_planes_N.npy) in
    the PlaneRCNN model.
     PlaneRCNN annotations for our dataset can be found [here](https://drive.google.com/file/d/1rmRO109i-zkM2iRxUZYs__n8FAPU0iqO/view?usp=sharing).
  
  You can refer to this [notebook](./notebook/Assignment15.ipynb) to get started.  
