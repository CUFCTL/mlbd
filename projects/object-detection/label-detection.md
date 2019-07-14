## Label Detection

This page provides information about the label dataset including how to download the dataset, the testing set, what is provided in these download as well as some of the things that should be done with the dataset.

  * <a href="#download">Download Datasets</a>
  * <a href="#augmentation">Data Augmentation Tasks</a>
  * <a href="#training">Training Labels</a>

<a name="download" />

### Downloading the dataset

This data was provided by BMW with the caveat that we do not distribute the data.  With that said, once you download the data, keep it on your local machine or on your Palmetto user account (don't post anywhere or make it available for anyone else to gain access, i.e. Github, etc.).  There are 2 easy ways to download these datasets (NOTE: the URL will only work if you are on the Clemson network, so it is necessary to either be on campus or use a VPN to retrieve the data):
  * Downloadable link <a href="http://130.127.248.49/BMW_Labels.zip">Download Dataset</a>, <a href="http://130.127.248.49/test.zip">Download Test Set</a>
  * Use wget to download dataset (This is good for downloading directly to Palmetto):
    * Download Dataset
      ```
      wget http://130.127.248.49/BMW_Labels.zip
      ```
    * Download Test Set
      ```
      wget http://130.127.248.49/test.zip
      ```

You will be required to enter a password once you have downloaded the dataset, which will be provided on a different channel (you will receive the password on the CUFCTLDLBD Slack Group).  The BMW_Labels should be used to develop your training set.  After training is complete, the `test` set can be used to determine how well your network performs.

<a name="augmentation" />

### Dataset augmentation/manipulation

Provided in this repository is a python script which will aide you in completing the task of data augmentation and manipulation.  The comments present in this file show where adding and modifying code is necessary to create the dataset.  However, before modifying this file, it may be necessary to simply test out some image processing techniques on the images.  The file can be found at [generate-data.py](https://github.com/CUFCTL/DLBD/blob/master/scripts/generate-data.py)

For *label manipulations*, you should begin thinking of types of image manipulation can be done and think of how each one could coincide with an actual scenario that would be encountered in a warehouse.  For example, you could perform Gaussian Blur on the labels before placing them on a background to simulate the camera not being completely steady when the detection system is running.  Feel free to go as in-depth with this study as you feel necessary (i.e. using multiple image processing techniques simultaneously or pipelined to achieve a different result).  Some helpful links that may point you in the right direction include:
  * <a href="http://docs.opencv.org/3.0-beta/doc/py_tutorials/py_tutorials.html">OpenCV Python Tutorials</a>
  * <a href="http://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_table_of_contents_imgproc/py_table_of_contents_imgproc.html#py-table-of-content-imgproc">Image Processing in OpenCV</a>
  * <a href="https://media.readthedocs.org/pdf/opencv-python-tutroals/latest/opencv-python-tutroals.pdf">OpenCV-Python Tutorials Documentation</a>

For this task you can simply take a few images from the dataset provided and perform the aforementioned image transformations on them.  This will give you an idea of how the labels will look once placed in the dataset.

Once you have some image processing techniques ready that you feel warrant creation of the entire dataset, you can move on to modifying the provided script.  You can modify it in a way to incorporate your chosen image processing techniques as well as make other modifications that will make your dataset unique.

As stated before, the comments in the file should help you read the code and decipher what is going on as well as lead you to the correct places for you add code.  A few of the things that you should add to the script for dataset generation include:
  * Background selections
  * Label manipulations
  * Other ways to modify the label/entire image for testing object detection networks

You are free to choose whichever backgrounds you would like for creating your dataset.  This may be handpicking backgrounds that you think mimic the warehouse or downloading a dataset (similar to what we already have shown) and using those images as the backgrounds.

<a name="training" />

### Training a YOLO network with the dataset

Once you have created your dataset (including image manipulations) with backgrounds, you can begin the training process.  The script provided in this repo [generate-data.py](https://github.com/CUFCTL/DLBD/blob/master/scripts/generate-data.py) should provide an entire dataset with accompanying labels for each image.  It may be simpler (and faster) to execute [generate-data.pbs](https://github.com/CUFCTL/DLBD/blob/master/scripts/generate-data.pbs) instead of the python file directly as this does not need an interactive node.  If you choose to use the `.pbs` file, simply change the <path to generate-data.py script> line in the file and then run `qsub generate-data.pbs`.  This will execute on the next available node and does not need interaction from the user.  After executing the `generate-data.py` script (or `generate-data.pbs`), your directories should be structured as follows (note: directory name vary, but here is `barcode-detection`):

```
barcode-detection
|-----train
      |-----images
      |-----labels
|-----val
      |-----images
      |-----labels
```

The images have been generated and split into a training and validation set (necessary for training a network correctly).  However, the data is currently written in a format known as KITTI format.  This format contains many parameters not needed for Darknet training including occlusion, truncation, alpha, etc.  Also, KITTI format gives bounding box coordinates for ground truths in the format `[x1, y1, x2, y2]` where these are the top left and bottom right corners of the box.  Darknet format requires `[x, y, width, height]` where `x` and `y` are the center coordinates of the bounding box and all fields are normalize (between 0.0 and 1.0).  A script has been provided ([convert-kitti-to-yolo.py](https://github.com/CUFCTL/DLBD/blob/master/scripts/convert-kitti-to-yolo.py)) which will perform the necessary conversion to begin training.  This script can be used as follows:
```
python convert-kitti-to-yolo.py <dataset-directory> <label-file>
```
where `dataset-directory` is the top-level directory containing train and val split directories and `label-file` is a text file containing a list of all classes (so for the BMW Label datasets, the text file will simply contain 1 line which says `Label`)

Once the labels have been converted into YOLO format (directory structure below), you are ready to follow the tutorial for training a network on your data.  The tutorial can be found at [Darknet on Palmetto](Darknet-on-Palmetto#training-custom).

```
barcode-detection
|-----train
      |-----images_kitti
      |-----images_yolo
      |-----labels_kitti
      |-----labels_yolo
|-----val
      |-----images_kitti
      |-----images_yolo
      |-----labels_kitti
      |-----labels_yolo
```
