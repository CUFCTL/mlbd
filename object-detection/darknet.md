## Darknet on Palmetto

Below you will find instructions on how to install Darknet and set all dependencies and modules correctly for use on the Palmetto Cluster.

  * <a href="#explanation">What is Darknet</a>
  * <a href="#module-add">Module Setup on Palmetto</a>
  * <a href="#installation">Installation of Darknet on Palmetto</a>
  * <a href="#inference">Testing Darknet/Inference</a>
  * <a href="#training">Training Darknet</a>
  * <a href="#training-custom">Training Darknet on Custom Dataset</a>
  * <a href="#testing-custom">Testing Custom Weights</a>
  * <a href="#evaluation">Evaluation of Trained Networks</a>

<a name="explanation" />

### What is Darknet?

Darknet is an open source neural network framework written in C and CUDA.  It is easy to install and supports CPU and GPU computation.  Some of the important features of Darknet include YOLO (Real-time object detection), Nightmare, and RNNs.  For this creative inquiry, we will be focusing solely on the YOLO piece of the framework; however, if you have interest to explore the others, you are more than welcome to at [Darknet](http://pjreddie.com/darknet/).

Since its inception, YOLO has been built upon leading to YOLOv2 which at the time of this writing is a state-of-the-art, real-time object detection system capable of processing 40-90 FPS with mAP on VOC 2007 of 78.6%.

#### How It Works

Apply a single neural network to the full image which divides the image into regions and predicts bounding boxes and probabilities for each region.  Bounding boxes are then weighted by predicted probabilities.

![](http://pjreddie.com/media/image/model2.png)

<a name="module-add" />

### Module Setup on Palmetto

No matter if you have already installed Darknet on Palmetto in your home directory or have not yet installed it, you will need to make sure to perform all computations (compilations) on a node that is not the user node (i.e. `user001`) as well as add all necessary modules.  Below is a set of commands that will obtain a node on Palmetto with GPGPU for you to work on (for a total of 6 hours).  You can either request a K20 or a K40 node (keeping in mind that the K40 has more memory than the K20).

```
# Request a K20 Node
qsub -X -I -v cores=8 -l select=1:ncpus=8:mem=10gb:ngpus=1:gpu_model=k20,walltime=6:00:00
# Request a K40 Node
qsub -X -I -v cores=8 -l select=1:ncpus=8:mem=10gb:ngpus=1:gpu_model=k40,walltime=6:00:00
```

If you can not obtain a node due to an error that says `No display set`, simply remove the `-X` flag from the command above and try again.  Once you have obtained a node with a GPGPU, you will need to add the appropriate modules for Darknet to work properly.  These modules may be added to your `.bashrc` file so they are loaded each time.

```
module purge
module add gcc/4.8.1 anaconda/4.2.0
module add opencv/2.4.9 ffmpeg/2.4
module add cuda-toolkit/8.0.44 cuDNN/8.0v5.1
```

After obtaining a node and making sure the appropriate modules are loaded you are ready to proceed.  If you have not yet 'installed' Darknet, continue to [Installation](#installation).  If you have already installed Darknet, continue to either [Training](#training) or [Inference](#inference).

<a name="installation" />

### Installation

Before trying to install Darknet on Palmetto, make sure to go through the short tutorial on accessing Palmetto which can be found at [[Palmetto Cluster]].  After reading this short documentation you are ready to install.

First make sure to obtain a node on Palmetto with a GPGPU and load all modules as explained in [Module Setup on Palmetto](#module-add).

### Setup

Clone the Darknet git repository.
```
git clone https://github.com/pjreddie/darknet.git
```

Next, `cd` into the newly created directory:
```
cd darknet
```

Now we need to edit the Makefile so that the compilation is performed with OpenCV, CUDA, and cuDNN.  Use your favorite text editor (hopefully vim) to open the Makefile and make the following changes.  You can simply replace the lines in the Makefile with those below.

Set GPU, CUDNN, and OPENCV flags to 1
```
GPU=1
CUDNN=1
OPENCV=1
DEBUG=0
```

Add compute architecture for newest architecture
```
ARCH=	-gencode arch=compute_35,code=sm_35
```

Set paths to palmetto installations of OpenCV, CUDA, and cuDNN
```
ifeq ($(OPENCV), 1)
COMMON+= -DOPENCV
CFLAGS+= -DOPENCV
LDFLAGS+= -L/software/opencv/2.4.9/lib `pkg-config --libs opencv`
COMMON+= -I/software/opencv/2.4.9/include `pkg-config --cflags opencv`
endif

ifeq ($(GPU), 1)
COMMON+= -DGPU -I/software/cuda-toolkit/8.0.44/include/
CFLAGS+= -DGPU
LDFLAGS+= -L/software/cuda-toolkit/8.0.44/lib64 -lcuda -lcudart -lcublas -lcurand
endif

ifeq ($(CUDNN), 1)
COMMON+= -DCUDNN -I/software/cuda-toolkit/cuDNN-8.0v5.1/include/
CFLAGS+= -DCUDNN
LDFLAGS+= -L/software/cuda-toolkit/cuDNN-8.0v5.1/lib64 -lcudnn
endif
```

### Building Darknet

After loading all appropriate modules (this will need to be done every time you log onto Palmetto), cloning the repository and editing the Makefile, the software can then be compiled.  Simply type `make` in the directory with the Makefile.  This process may take a few minutes since we are building with support for OpenCV, CUDA, and cuDNN.

<a name="inference" />

### Testing and Execution of Darknet

Once you have successfully created you environment on a GPGPU node with the correct modules you can test your installation by issuing the following command from the Darknet home directory:

```
./darknet
# Output
usage ./darknet <function>
```

#### Run a few examples

Darknet does not come pre-installed with any weights or models that can be used for inference; therefore before running any examples, a set of weights and a model file must be downloaded and placed in the correct directory.  We will be using the yolo.weights file to run the examples.  To download the weights to the correct location (after cloning the Darknet repository and building it) run the following set of commands:

```
# Create a directory for the weights and change to that directory
mkdir /home/${USER}/darknet/weights
cd /home/${USER}/darknet/weights

# Download the weights from pjreddie's website
wget http://pjreddie.com/media/files/yolo.weights
```

With the weights file downloaded (and model file already available in `cfg` directory), can we now run a few tests and make sure that it works correctly.  To use the pre-trained network to run a simple object detection, use the following command:

```
# Change to the darknet directory
cd /home/${USER}/darknet

# Run the detector
./darknet detector test cfg/coco.data cfg/yolo.cfg weights/yolo.weights data/dog.jpg
```

By running the YOLO detector you will get a similar output to the following:

```
layer     filters    size              input                output
    0 conv     32  3 x 3 / 1   416 x 416 x   3   ->   416 x 416 x  32
    1 max          2 x 2 / 2   416 x 416 x  32   ->   208 x 208 x  32
    2 conv     64  3 x 3 / 1   208 x 208 x  32   ->   208 x 208 x  64
    ........
   28 conv   1024  3 x 3 / 1    13 x  13 x3072   ->    13 x  13 x1024
   29 conv    425  1 x 1 / 1    13 x  13 x1024   ->    13 x  13 x 425
   30 detection
Loading weights from weights/yolo.weights...Done!
data/dog.jpg: Predicted in 0.050000 seconds.
car: 54%
bicycle: 51%
dog: 56%
```

![](http://pjreddie.com/media/image/Screen_Shot_2016-11-17_at_11.14.54_AM.png)

Darknet prints objects which are detected, the confidence in those objects, and how long it takes to make the predictions for each object.  Since we compiled with OpenCV (and assuming you have started your Palmetto session with the -X flag for display), you will see an image displayed with the original image and the detections.  It also saves the result in `predictions.png` which is the output if you don't compile Darknet with OpenCV as well.  You can copy the `predictions.png` using FileZilla (or equivalent) to your machine to view the detections.  On the K40 nodes you should get about 0.05 seconds for inference time (prediction time).

Included with the repo are a few more images that you can try with this model including `data/eagle.jpg`, `data/dog.jpg`, `data/person.jpg`, and `data/horses.jpg`.

#### Change Detection Threshold

By default, YOLO displays objects detected at a confidence of .25 or higher.  If some objects are not getting detected in your images, you can pass the `-thresh <val>` flag to the `detector` command to display more bounding boxes (<val> is normalized between 0.0 and 1.0).  This may allow for more objects to get picked up but will also draw boxes that are false positives.  For example, if the threshold is set to 0:

```
./darknet detector test cfg/coco.data cfg/yolo.cfg weights/yolo.weights data/dog.jpg -thresh 0
```

With the threshold set to 0, every bounding box that is generated (even with a probability of 0) is displayed and looks as follows:

![](http://pjreddie.com/media/image/Screen_Shot_2016-11-17_at_12.03.22_PM.png)

#### Use your own images

We don't always want to use the test images that come with the build; we are more interested in seeing how the model performs on our custom data.  For this we can create another directory on Palmetto for personal images and use them for processing.  You can simply create a directory on Palmetto (preferably in your personal scratch2 directory) and move all the images to that location.

```
# On Palmetto
mkdir /scratch2/${USER}/yolo-test-images
```

After the directory for images is created on Palmetto, you can use either FileZilla or `scp` to copy the images to test to Palmetto.  An examples `scp` command to copy a given image to the created directory is shown here:

```
scp <path-to-file>/<filename> <username>@user.palmetto.clemson.edu:/scratch2/<username>/yolo-test-images/
```

Once the files are present on the Palmetto, the same command from above can be used for inference on these images.  The following example assumes an image named car1.png was uploaded to the scratch2 directory.

```
# Change to darknet directory
cd /home/${USER}/darknet

# Run the detector
./darknet detector test cfg/coco.data cfg/yolo.cfg weights/yolo.weights /scratch2/${USER}/yolo-test-images/car1.png
```

<a name="training" />

### Training YOLO

You can train YOLO from scratch if you would like to experiment with the different datasets, models, etc.

#### Train on Pascal VOC

To train YOLO on VOC, first retrieve all the VOC data for 2007 and 2012 from the following links (use the following commands to download them onto you machine, whether that is a local machine or Palmetto):

```
wget http://pjreddie.com/media/files/VOCtrainval_11-May-2012.tar
wget http://pjreddie.com/media/files/VOCtrainval_06-Nov-2007.tar
wget http://pjreddie.com/media/files/VOCtest_06-Nov-2007.tar
tar xf VOCtrainval_11-May-2012.tar
tar xf VOCtrainval_06-Nov-2007.tar
tar xf VOCtest_06-Nov-2007.tar
```

After download all these and untar-ing them, you will now have a `VOCdevkit` subdirectory with all of the training data located inside of it.

Next we need to generate the label files needed to train Darknet which is in the form of a `.txt` file corresponding to each image with a line for each of the objects in the image.  The label file format is as follows:

```
<object-class> <x> <y> <width> <height>
```

Where `x`, `y`, `width`, and `height` are relative to the image's width and height.  You can easily generate these labels by running the file `voc_label.py` which is provided in the Darknet repository or can be downloaded and executed as follows:

```
curl -O http://pjreddie.com/media/files/voc_label.py
python voc_label.py
```

This script will generate labels in `VOCdevkit/VOC2007/labels/` and `VOCdevkit/VOC2012/labels/` as well as text files containing a list of the image files in each directory.  We would like to use all the images in both to train; therefore, we should create one `.txt` file with all training images.

```
cat 2007_train.txt 2007_val.txt 2012_*.txt > train.txt
```

This is all that needs to be done for data setup.  Now we can set up the configuration and model files needed for training.  In the Darknet directory, there should be a file `cfg/voc.data` that should be modified to point to the data that you just downloaded and created.

``` 
  1 classes= 20
  2 train  = <path-to-voc>/train.txt
  3 valid  = <path-to-voc>2007_test.txt
  4 names = data/voc.names
  5 backup = backup
```

You can then download the pretrained [Extraction](http://pjreddie.com/darknet/imagenet/#extraction) model which will help the training converge quicker.  If the weights are not present already, you can download them by:

```
curl -O http://pjreddie.com/media/files/darknet19_448.conv.23
```

Now we are ready to train the network.  The following command can be invoked to start training the network on the newly created dataset along with configuration files that were just modified.

```
./darknet detector train cfg/voc.data cfg/yolo-voc.cfg darknet19_448.conv.23
```

#### Train on Multiple GPUs

Training on multiple GPUs is possible, but should only be done once you are confident training on a single GPU as it involves training on a single GPU for a small number of iterations.

1. First train on a single GPU for about 1000 iterations to get a base pretrained model: `./darknet detector train data/voc.data yolo-voc.cfg darknet19_448.conv.23`
2. Halt the training and use the partially trained model (`backup/yolo-voc_1000.weights`) for multi-gpu training: `./darknet detector train data/voc.data yolo-voc.cfg yolo-voc_1000.weights -gpus 0,1,2,3`

Note this assumes you have 4 GPUs.  If you only have 2, simply use `-gpus 0,1` flag.

<a name="training-custom" />

#### Train on Custom Data

For this explanation, the dataset name will be named `custom` and all example files explained below will be set accordingly.  Appropriate changes to file names will be necessary.  To train a YOLO network on a custom dataset, the following steps must be accomplished.

1. Create a model file `yolo-custom.cfg` containing the network architecture with the same contents as in `yolo-voc.cfg` (or any other model as a starting point).  This can be done by simply copying `yolo-voc.cfg` and renaming it.  Inside `yolo-custom.cfg` make the following changes:

    * change `classes=20` near the bottom of the model file to your number of classes
    * change `filters=125` to `filters=(classes + coords + 1)*num` where coords, num, and classes are defined in the model file already (right below).  Calculate the number of filters for your specific dataset.

    For example, for 10 objects, the changed parameters in the model file should be
    ```
    [convolutional]
    filters=75

    [region]
    classes=10
    ``` 

2. Create a label file `custom.names` with each line containing one of the classes.  This is the same file that was created when using the `convert-kitti-to-yolo.py` script before.
3. Create a data file `custom.data` containing information about the training/val images, etc.
   
    ```
    classes = 10        # Change this to the number of classes in your dataset
    train   = train.txt # Change this to the path of train.txt (created by `convert-kitti-to-yolo.py`)
    valid   = val.txt   # Change this to the path of val.txt (created by `convert-kitti-to-yolo.py`)
    names   = custom.names # Change this to the path of the file created in (2) (i.e. data/custom.names)
    backup  = backup/   # Directory where all intermediate weights generated during training are stored
    ```

4. Make sure all `.jpg` images are located in the directory where train.txt/val.txt illustrate (images-yolo).  This should be done by `convert-kitti-to-yolo.py`.  Text files for each `.jpg` image should have also been generated (labels-yolo).  Each label file should contain `<object-class> <x> <y> <width> <height>`
    
    Where:
    * `<object-class>` - integer number of object from `0` to `(classes-1)`
    * `<x> <y> <width> <height>` - float values relative to width/height of image
    * example: `<x> = <absolute_x> / <image_width>` or `<height> = <absolute_height> / <image_height>`
    * NOTE: `<x> <y>` are the center of the detection box (not the top-left corner)

    An example of a label file for an image that contains 2 instance of object 0 and 1 instance of object 1 is:
    ```
    0 0.716797 0.395833 0.216406 0.147222
    1 0.687109 0.379167 0.255469 0.158333
    0 0.420312 0.395833 0.140625 0.166667
    ```

    Text files `train.txt` and `val.txt` should have also been created enumerating the contents of the image folders and should appear as follow (with the path the images being `/data/custom/`):
    ```
    /data/custom/train/images-yolo/img1.jpg
    /data/custom/train/images-yolo/img2.jpg
    ...
    /data/custom/val/images-yolo/img1.jpg
    /data/custom/val/images-yolo/img2.jpg
    ```

5. If not already downloaded, download pretrained weights from [Darknet_448](http://pjreddie.com/media/files/darknet19_448.conv.23)

6. Begin training by invoking the following command with all the files just created:
   
    ```
    ./darknet detector train data/custom.data cfg/yolo-custom.cfg weights/darknet19_448.conv.23
    ```

    It may also be helpful to use the [train-darknet.pbs](https://github.com/CUFCTL/DLBD/blob/master/scripts/train-darknet.pbs) script to run on a non-interactive node.  This allows you to train without making sure the session doesn't get killed.  Make sure to change the `.pbs` script to reflect the paths to each of your files you create.

  After training is complete (or during training), resulting weights to be used for inference can be found in the `backup/` directory.

<a name="testing-custom" />

#### Testing Custom Weights

Once you have trained your own set of weights, you can now easily test your network to see how well it did.  Simply use the files that you generated for training with the files generated during training to test.  An example is show below:

```
./darknet detector test data/custom.data cfg/yolo-custom.cfg backup/yolo-custom_final.weights <path-to-test-image.jpg>
```

<a name="evaluation" />

#### Evaluation of Trained Networks

Once you have trained a network, it is necessary to evaluate its performance.  The set of evaluation metrics that we will use include (but are not limited to) IoU, recall, and precision.

![](https://camo.githubusercontent.com/ffd00e8c7f54d4710edea3bb47e201c8bedab074/68747470733a2f2f6873746f2e6f72672f66696c65732f6361382f3836362f6437362f63613838363664373666623834303232383934306462663434326137663036612e6a7067)

*Image courtesy of https://github.com/AlexeyAB/darknet*

To evaluate a set of trained weights for recall and IoU, we will first need a list of validation images.  This should have been created by `convert-kitti-to-yolo.py` and is called `val.txt` inside your dataset directory.  Simply copy the full path to this file and then open `src/detector.c` in an editor.  In the `validate_detector_recall` function there is a line that appears as follows:

```
list *plist = get_paths("data/voc.2007.test");
```

Simply change this to the full path to your list of validation image (`val.txt`).

```
list *plist = get_paths("/data/custom/val.txt");
```

Another option is to use the <a href="http://130.127.248.49/test.zip">Test Set</a> for evaluation.  Simply unzip the file and inside the `test/images/` directory, execute the following command:

```
ls -d -1 $PWD/*.* > custom.test
```

If you choose this test set, simply replace `data/voc.2007.test` with `<full-path>/custom.test` above in `src/detector.c`.

Since we have made changes to the code, we must now build it again (don't worry it will only build the file you changed).  Simply type `make` in the top-level Darknet directory with the Makefile in it.

Once you have rebuilt darknet, you can now evaluate the performance using the recall and IoU metrics as follows:

```
./darknet detector recall data/custom.data cfg/yolo-custom.cfg backup/yolo-custom_final.weights
```

This goes through each image in the validation directory and calculates IoU and recall.  The final line of the execution should appear as follows (giving you the final recall and IoU for the validation set):

```
1124  1990  2306  RPs/Img: 94.14  IOU: 68.97%  Recall:86.30%
```
