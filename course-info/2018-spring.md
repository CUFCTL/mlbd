# Spring 2018

Below you will find information about the projects and software used during the Spring 2018 semester of the DLBD Creative Inquiry.

| Week | Topic | Deliverable |
|:----:|:---------------------------------:|:----------------------------------:|
| 1 | <a href="#intro">Introduction/Semester Explanation</a> | Installation of TF API on Palmetto |
| 2 | <a href="#neuralnetintro">Dataset Creation and Synthesis</a> | Create TFRecords for 1-class data |
| 3 | <a href="#training">Training Overview</a> | Trained network for 1-class data |
| 4 | <a href="#train_inf">Training and Inference Details</a> | Have .ckpt files from training ready |
| 5 | <a href="#inference">Freeze Model/Inference Details</a> | Use frozen model for inference |
| 6 | <a href="#evaluation">Evaluation of Models</a>/<a href="#ass1">Assignment 1</a> | Performance of trained model |
| 7 | <a href="#convolution">Introduction to Convolution</a> | Continue training 20-class Networks |
| 8 | <a href="#optimization">Optimization Techniques</a> | Try Different Hyperparameters w/ Training |
| 9 | <a href="#qwiklab">Qwiklab Introduction</a> | Object Detection Qwiklab Notebook |
| 10 | **Spring Break** | **Spring Break** |
| 11 | Working Week | 20-Class Detection/Qwiklab |
| 12 | <a href="#final">Final Project Assignment</a> | 20-class Evaluation/Start Final Project |
| 13 | Working Week | Chosen Dataset/Created TFRecords/Begin Training |
| 14 | <a href="#latex">Latex Introduction | Continue project/Begin report |
| 15 | <a href="#wrapup">CI Wrap-up</a> | Final Report Due (Apr. 27) |

* <a href="#steps">Every Time you log into Palmetto</a>
* <a href="#faq">Frequently Asked Questions (FAQ)</a>

<a name="intro" />

# Introduction

[Intro Presentation](https://drive.google.com/open?id=1olFKwxPOCn-sVBnxpGVUAbXREQnkAZcJ "CI Introduction Presentation")

Throughout the course of the semester, you will train and evaluate numerous networks for object detection on self-crafted datasets, well-known public datasets, and a few research-related datasets.  The [Tensorflow Object Detection API](https://github.com/tensorflow/models/tree/master/research/object_detection) will be used to aide the training and evaluation processes for your neural networks.

You will be using the [Palmetto Cluster](https://www.palmetto.clemson.edu/palmetto/) for all training and evaluation.  If you have a GPU in your machine and would like to try setting up the environment on your machine feel free; however, training on a machine without a GPU will be infeasible due to the size of some of the models.  If you do not have an account for the Palmetto Cluster go [here](http://citi.clemson.edu/new-account/) before proceeding.  Once you are able to log onto the cluster, follow the steps to create your environment and setup modules for training.

## Setup on Palmetto Cluster Environment

*Note: Setup should only need to occur once.  Skip this step if you have already set up everything.*

Retrieve a node with a GPU

    qsub -I -v cores=20 -l select=1:ncpus=20:mem=16gb:ngpus=2:gpu_model=k40,walltime=48:00:00

Add necessary modules

    # Note: these can be added to the .bashrc (so as not to have to add them every time)
    module add anaconda3/4.3.0
    module add cuda-toolkit/8.0.44
    module add cuDNN/8.0v6

Build the Protobuf Compiler (protoc)

    mkdir ${HOME}/bin/protoc -p
    curl -OL https://github.com/google/protobuf/releases/download/v3.2.0/protoc-3.2.0-linux-x86_64.zip
    unzip protoc-3.2.0-linux-x86_64.zip -d ${HOME}/bin/protoc/
    rm protoc-3.2.0-linux-x86_64.zip

Create an Anaconda Environment

    # Create conda environment
    conda create -n ci-py35 python=3.5 --yes

    # Activate the created environment
    source activate ci-py35

    # Install necessary python packages
    pip install tensorflow-gpu==1.4
    pip install jupyter
    pip install matplotlib
    pip install numpy
    pip install lxml
    pip install Pillow
    pip install halo
    pip install opencv-python

## Setup Tensorflow Object Detection API

Clone the Tensorflow models repository (containing the object detection API) along with many other projects being explored by Google.  Feel free to explore other projects in your free time if you are interested.

    git clone https://github.com/tensorflow/models.git ci-models

Add the following paths to your ~/.bashrc file:

    export PATH=${PATH}:${HOME}/bin/protoc/bin
    export PYTHONPATH=${PYTHONPATH}:${HOME}/ci-models/research
    export PYTHONPATH=${PYTHONPATH}:${HOME}/ci-models/research/slim

Source your .bashrc file so environment variables are updated

    source ~/.bashrc

With the Anaconda environment activated, compile the Protobuf libraries

    cd ${HOME}/ci-models/research
    protoc object_detection/protos/*.proto --python_out=.
    # NOTE: This should produce no output if everything is correct

Test to make sure the Protobuf compiler is installed correctly

    cd ${HOME}/ci-models/research
    python object_detection/builders/model_builder_test.py

    # The result should be as follows (or something similar)
    .......

    ----------------------------------------------------------------------
    Ran 7 tests in 0.044s

    OK

<a name="steps" />

# Every Login to Palmetto

The following commands should be run every time you log onto Palmetto:

1. Retrieve a node with a GPU (DO NOT run code on login node)

        qsub -I -v cores=20 -l select=1:ncpus=20:mem=16gb:ngpus=2:gpu_model=k40,walltime=48:00:00

2. If you haven't added the modules to your ~/.bashrc (which is sourced every time you open a new session)

        module add anaconda3/4.3.0
        module add cuda-toolkit/8.0.44
        module add cuDNN/8.0v6

3. Activate the Anaconda environment you created

        source activate ci-py35

This should leave you on a command prompt beginning with `(ci-py35)`.


<a name="neuralnetintro" />

# Neural Network Introduction and Dataset Creation

[Neural Net/Dataset Presentation](https://drive.google.com/open?id=1mcTOq5zcILwT0pHvaYE-2b287u7qVMKn "CI Neural Network and Dataset Presentation")

First, you will take a small portion of a given dataset and perform what is called `labeling`.  For a large dataset, this is a tedious task, but you will only need to label 50-100 images.  To do this you will use [labelImg](https://github.com/tzutalin/labelImg), a tool designed to make labeling much easier.  Simply download the dataset given to you and open the directory with the `labelImg` tool.  You will then be able to draw bounding boxes on the image.  Once you are satisfied with the bounding boxes you have drawn simply click save and it will save the `label` file into what is known as [PASCAL VOC format](http://host.robots.ox.ac.uk/pascal/VOC/).  This is the format you will need to start with for the training of the data.

Retrieve the images with the following command:

    wget http://eceftl10.ces.clemson.edu/<class-name>.zip

where <class-name> should be the class that is given to you.  The following chart will give you the class you will be dealing with since everyone will have a different object to start with.

| Name           | Class (Object) |
|----------------|:--------------:|
| Charles Carter |      boat2     |
| Keith Hammock  |      car1      |
| David Langbehn |      car18     |
| Lane Enloe     |      car5      |
| Ryan Macrae    |     person1    |
| Michael Nelson |    person24    |
| Casey Sumner   |     person6    |
| Tianyi Zhang   |     person8    |


The Tensorflow Object Detection API uses the [TFRecord file format](https://www.tensorflow.org/api_guides/python/python_io#tfrecords_format_details), so the first thing that needs to be done before any type of training is making sure your dataset is converted into this format.  Since the labels are already in PASCAL VOC format, you can simply run a set of scripts that will place the files in the correct directories, create the training and validation set split, and then create the TFRecords needed for training.  Copy the following files to the given locations and then execute the following commands:

Make sure your images and annotations (XML files) are in the following directory: `${HOME}/ci-models/data/${CLASS}` where class is the name of the object you were given to identify (i.e. car4 or person 7)

Copy the following files to the ci-models directory from [Spring2018/src](https://github.com/CUFCTL/DLBD/tree/master/Spring2018/src):

* `convert_data.py`
* `create_trainval_set.py`
* `create_ci_tfrecords.py`
* `utils.py`

        # Convert the data to have the correct file structure
        # From ci-models directory
        python convert_data.py \
            --ci_path ${HOME}/ci-models/data/ \
            --out_path ${HOME}/ci-models/data/CIdevkit/

This will produce a directory called CIdevkit within which will be another directory called CI2018.  Inside this directory, you can find all the images and annotations that were created for your dataset.  This will also produce a label map file (.pbtxt) in the `data` directory which is a structured file which holds the classes that should be detected by the network.  The following is a simple example of an entry in the label map file:

    item {
      id: 1
      name: 'car'
    }

> NOTE: It's important that your label map file starts from id 1 as index 0 is used as a placeholder index.

    # Split the data into training and validation
    # From ci-models directory
    python create_trainval_set.py \
        --out_path ${HOME}/ci-models/data/CIdevkit/ \
        --train 0.75 \
        --val 0.25

This will produce a `train.txt` and a `val.txt` listing the images that will be used for training and those that will be used for validation.

    # Create the TFRecords from the newly created training and validation split
    # Create directory for tfrecords to be created in
    mkdir ${HOME}/ci-models/tfrecords/

    # From ci-models directory
    python create_ci_tfrecords.py \
        --label_map_path=data/ci_label_map.pbtxt \
        --data_dir=${HOME}/ci-models/data/CIdevkit/ \
        --year=CI2018 \
        --set=train \
        --output_path=${HOME}/ci-models/tfrecords/ci_train.record

    python create_ci_tfrecords.py \
        --label_map_path=data/ci_label_map.pbtxt \
        --data_dir=${HOME}/ci-models/data/CIdevkit/ \
        --year=CI2018 \
        --set=val \
        --output_path=${HOME}/ci-models/tfrecords/ci_val.record

<a name="training" />

# Training a Neural Network

Once you have successfully created the TF Record files, there are a few other things that you will need to configure before you are ready to train your first network.

* [Create a configuration file](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/configuring_jobs.md) that will contain all "hyper-parameters" for your training.  There are some sample configuration files provided in the `ci-models` repository that you have downloaded (located in `ci-models/research/object_detection/samples/configs/`).  Located in theses there are alot of options that can be changed that will effect the outcome of your training job.  For the purposes of this tutorial, only a few changes need to be made to the configuration files:
  * `num_classes` should be changed to `1` for this particular experiment (should match the number of classes in your dataset)
  * Any instance of `PATH_TO_BE_CONFIGURED` for the model_checkpoint, input_path, and label_map_path (which should point to the TFRecord and .pbtxt files you created earlier in the tutorial.
* It is recommended to use a pre-trained model to start your training instead of initializing the network with random weights.  There are many pre-trained models that are provided at [Object Detection Model Zoo](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md).  When choosing a model, make sure that you choose the model corresponding to the sample configuration file you chose to modify.  Once you download the pre-trained model, the `fine_tune_model` in the configuration file should be set to the path of this pre-trained checkpoint.

Now that you are familiar with what is needed, here are the steps to perform the aforementioned tasks and begin a training job:

1. Choose a configuration file that you would like to modify and copy it to `ci-models/data/` directory.  I would recommend for your first attempt to use one of the SSD models (i.e. SSD_inception or SSD_mobilenet).  Rename the file so that it has a meaningful name.

        # From ${HOME}/ci-models directory
        cp research/object_detection/samples/configs/ssd_inception_v2_coco.config data/ssd_inception_v2_coco_ci.config
        # OR
        cp research/object_detection/samples/configs/ssd_mobilenet_v1_coco.config data/ssd_mobilenet_v1_coco_ci.config

2. Make a directory for pre-trained models and download one of the pre-trained models from the zoo

        # From ${HOME}/ci-models directory
        mkdir pretrained
        cd pretrained
        
        wget http://download.tensorflow.org/models/object_detection/ssd_inception_v2_coco_2017_11_17.tar.gz
        # OR 
        wget http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_2017_11_17.tar.gz

3. Modify `num_classes` and `PATH_TO_BE_CONFIGURED` variables in your new configuration file.

Now you are ready to train your first network.

    # From ${HOME}/ci-models directory
    python research/object_detection/train.py \
        --logtostderr \
        --pipeline_config_path=${PIPELINE_CONFIG} \
        --train_dir=${TRAIN_DIR}

where ${PIPELINE_CONFIG} is the recently created configuration file and ${TRAIN_DIR} is the directory where all the training [checkpoints](https://www.tensorflow.org/get_started/checkpoints) will be saved.

> NOTE: Training will sometime take more than a few hours, so either budget enough time to leave your terminal active or follow the instructions below to submit a non-interactive job to Palmetto for training.

## Training PBS Script

As noted above, sometime training can take hours or days (depending on the size of the dataset, and other hyper-parameters).  Therefore, this is not conducive to having an interactive session for training.  The Palmetto Cluster has a scheduling system which allows users to setup `job` files called `PBS scripts` that will run in on a node in the background.  A `PBS script` should contain all the information needed to not only run your code but also set up the environment to run the code correctly.  An example of a `PBS script` is provided [here](https://github.com/CUFCTL/DLBD/blob/master/Spring2018/src/train.pbs).  To submit this job to the Palmetto cluster, from the login node, simply run

    qsub train.pbs

> NOTE: You will need to edit the PBS script appropriately to match the paths to your files and data as well as modify the previous command if the script isn't in your current directory.

If all goes well, in the directory from which you submitted the job, you will have an output file called something similar to `train_tf.o#######`.  If something goes wrong during training, you will have a file called `train_tf.e#######` that will have the error printed inside of it.

<a name="train_inf" />

# Training and Inference Details

[Training & Inference Presentation](https://drive.google.com/open?id=1LpdywjalD_Q-QTeq-KcLMVSilnvGUn4P "CI Training and Inference Detailed Description")

<a name="inference" />

# Freeze Model/Inference Details

Once you have a completely trained network there are a few things that you will have produced to be used with `evaluation (val)` and `inference`.  First in `${TRAIN_DIR}`(the same path from the training command), you will see `checkpoint`, `event`, `graph.pbtxt`, and `model.ckpt-######.{data,meta,index}`.  These files will allow you to not only view the training process, but also create a **frozen graph** to perform inference.

## Creating a frozen graph

After your model has been trained, you should export it to a Tensorflow graph proto. A checkpoint will typically consist of three files:

* model.ckpt-${CHECKPOINT_NUMBER}.data-00000-of-00001
* model.ckpt-${CHECKPOINT_NUMBER}.index
* model.ckpt-${CHECKPOINT_NUMBER}.meta

Once you have chosen a checkpoint to export (typically, the one with the largest CHECKPOINT_NUMBER), you will run the following command to export the proto model (in .pb format):

    # From ${HOME}/ci-models directory
    python research/object_detection/export_inference_graph.py \
        --input_type image_tensor \
        --pipeline_config_path ${PIPELINE_CONFIG_PATH} \
        --trained_checkpoint_prefix ${TRAIN_PATH} \
        --output_directory output_inference_graph.pb

where `pipeline_config_path` points to the `.config` file that was used for training, `trained_checkpoint` is the path to the `model.ckpt` files, and `output_directory` is where the proto graph should be written.  The following is an example on command:

    # From ${HOME}/ci-models directory
    python research/object_detection/export_inference_graph.py \
        --input_type image_tensor \
        --pipeline_config_path /home/${USER}/ci-models/data/ssd_mobilenet_v1_coco_ci.config \
        --trained_checkpoint_prefix /home/${USER}ci-models/data/train_ckpts/ssd_mobilenet_v1_coco_ci/model.ckpt-200000 \
        --output_directory trained_models/ssd_mobilenet_v1_coco_ci

This will produce a directory called `trained_models/ssd_mobilenet_v1_coco_ci` in your `${HOME}/ci-models` directory.  Inside this directory will be a `frozen_inference_graph.pb` file containing the weights for your neural network as well as architectural and hyperparameter information (taken from the `.config` files).  You will not be able to view this file (as it's a binary file), but you can use it to run inference to see if the model was created correctly and if the model has been trained correctly.

## Using your Anaconda environment with JupyterHub

Before logging onto Palmetto's JupyterHub, a file similar to `~/.bashrc` should be generated that will give allow you to access your Anaconda environment inside JupyterHub.  First you will need to add the correct modules to the file `~/.jhubrc`:

    module add anaconda4/3.4.0     # Or whichever Anaconda you chose for installation
    module add cuda-toolkit/8.0.44 # Or whichever CUDA version was used for installation
    module add cuDNN/8.0v6         # Or whichever cuDNN version was used for installation

and then make sure to activate the environment:

    source activate ci-py35

With your Anaconda environment activated, you should `export the kernel` for JupyterHub to use as follows:

    python -m ipykernel install --user --name ci-py35 --display-name "Python (ci-py35)"

## Using Jupyter notebook for **inference**

With the modules loaded into `~/.jhubrc` file, the kernel added for Jupyter to be able to access it, and a Tensorflow proto format file (`.pb`) created, you are now ready to open Jupyter and modify the notebook for inference.  To access Palmetto's JupyterHub, go to the following [link](https://www.palmetto.clemson.edu/jupyterhub) and after logging in and pressing the Green `Start My Server`, the following menu should come up:

![](images/jupyterhub_login.png)

There you will select the following options (these can be changed; however, this is the recommended setup):

* **Number of resource chunks** - 1
* **CPU cores per chunk** - 4+
* **Amount of memory per chunk** - 6gb+
* **Number of GPUs per chunk** - 2
* **Walltime of notebook server** - 2hours+
* **Queue** - workq

Once logged into JupyterHub, you should be viewing your home directory (`/home/${USER}`).  Navigate into the directory `ci-models/research/object_detection/` and there should be jupyter notebook called `object_detection_tutorial.ipynb`.  This is the notebook that will allow you to run inference using your model.

Open this notebook and you will need to change the following parameters (in the following cells):

1. Select the kernel corresponding to your Anaconda environment in __Kernel__ -> __Change Kernel__
2. Run `Imports` cell and make sure there are no errors (this will import all necessary packages)
3. Run `Env setup` cell which will allow you to use the `object_detection` code built into the Tensorflow API
4. Run `Object detection imports` cell for loading the utilities for visualiation
5. Edit the `Variables` cell to point `PATH_TO_CKPT` to the `frozen_inference_graph.pb` that was created with `export_inference_graph.py`, `PATH_TO_LABELS` to the `ci_label_map.pbtxt` file that was generated with `convert_data.py`, and `NUM_CLASSES` to 1. Run this cell.
7. Skip running the `Download Model` cell as you have generated the model; there is no need to download
6. Run `Load a (frozen) Tensorflow model into memory` which will ready your `.pb` file and load it for inference
7. Run `Loading label map` and `Helper Code` (no changes necessary)
8. Change `PATH_TO_TEST_IMAGES_DIR` to a path with a ~5 images that you have taken from the dataset for testing.  Make sure these images have been renamed `image{}.jpg` where `{}` is replaced by 1, 2, etc.  Run this cell.
    > NOTE: A script is provided, [make_inf_set.py](https://github.com/CUFCTL/DLBD/blob/master/Spring2018/src/make_inf_set.py) to take random images from the dataset.
9.  Run the final cell, wait for a few seconds and you should see your images with a `green` bounding box where the model has found the object.

<a name="evaluation" />

# Evaluation

Evaluation is the process of taking a set of images (in this case the `validation set`) and performing inference on each one, recording the accuracy value, and then averaging over the entire set.  This is one good metric to determine if your network is performing well.  Since you have split the dataset into `training` and `validation` (i.e. `ci_train.record` and `ci_val.record`), the evaluation process will determine how well your network **generalizes** to new data since it has not been trained on the images in the `validation` set.  The evaluation job will periodically poll the training directory for new checkpoints (if run while training occurs); otherwise, it will use the latest checkpoint as the model for inference.

Depending on the Tensorflow version (and API version) that you have you may need to install the [COCO API](https://github.com/cocodataset/cocoapi) to run evaluation properly.  Just to be on the safe side, go ahead and install for all version.  The following steps will install the API on Palmetto in your home directory:

    # With your Anaconda environment activated
    pip install Cython
    # Clone the COCO API repository
    git clone https://github.com/cocodataset/cocoapi ${HOME}/cocoapi
    # 'Make' the code in the repository
    cd ${HOME}/cocoapi/PythonAPI
    make
    # Add the following path to your ~/.bashrc file (and source the file if necessary)
    export PYTHONPATH=${PYTHONPATH}:${HOME}/cocoapi/PythonAPI

After the COCO API has been installed and linked in the PYTHONPATH environment variable, you will need to add the following code before running evaluation (to print out the accuracy values for each class) in the file `ci-models/research/object_detection/eval.py`.

```python
import logging
logging.basicConfig(level=logging.INFO)
tf.logging.set_verbosity(tf.logging.INFO)
```

By default, `evaluation` is run 10 times during the process (this is just incase you are running evaluation during training).  Since we are running evaluation only once on the final trained model, we only want it to try and run once.  Therefore, you should edit the configuration file to reflect this (i.e. edit `data/ssd_inception...`) in the following way:

> **NOTE: The `-` means to delete the code whereas the `+` means to add the code.**

    # In `eval_config`
    - max_evals: 10
    + max_evals: 1

Now evaluation can be run with using the following python script.

> NOTE: Depending on the size of your validation set, this process could take a little while

    # From ci-models directory
    python research/object_detection/eval.py \
        --logtostderr \
        --pipeline_config_path=${PIPELINE_CONFIG_PATH} \ # Configuration file from training
        --checkpoint_dir=${TRAIN_DIR} \                  # Directory with training checkpoints
        --eval_dir=${EVAL_DIR}                           # Directory where __events__ will be stored

An example of a `PBS script` is provided [here](https://github.com/CUFCTL/DLBD/blob/master/Spring2018/src/eval.pbs).  This will produce a directory (specified with ${EVAL_DIR} containing `event` files which we can then view with Tensorboard.  To submit this job to the Palmetto cluster, **from the login node**, simply run

    qsub eval.pbs

> NOTE: You will need to edit the PBS script appropriately to match the paths to your files and data as well as modify the previous command if the script isn't in your current directory.

If all goes well, in the directory from which you submitted the job, you will have an output file called something similar to eval_tf.o#######; inside this file you will find the evaluation accuracy as well as the entire evaluation process displayed. If something goes wrong during evaluation, you will have a file called eval.e####### that will have the error printed inside of it.

Most likely, you will get 100% or close to 100% classification IOU meaning that you're network is 100% positive about every image that it has seen about the class; this, however, is going to be the case only with a single class dataset.  By adding more classes for the network to determine, you are adding complexity to the model.  This will result in lower accuracy values for each class (hopefully not too much lower if trained well).


<a name="ass1" />

# Assignment 1 Description

For the first larger project, you will take a slightly larger dataset (i.e. 20 classes) taken from the same dataset which you were given a single class before.  The suggestion is to place the new images in a directory inside `ci-models` separate from the data for the first (1-class) project.

    # Make a directory for the new data
    mkdir ${HOME}/ci-models/dac-data
    
    # Navigate to this directory
    cd ${HOME}/ci-models/dac-data
    
    # Download the dataset of images and labels (XML files)
    wget http://eceftl10.ces.clemson.edu/20-class.zip
    
    # Unzip the 20-class dataset into this directory (should create 20-class directory)
    unzip 20-class.zip
    
    # Remove the .zip file (if extracted properly)
    rm 20-class.zip

Now you are ready to work on the dataset.  The same process as before should be followed in order to create TFRecords, a `.pbtxt` file (for labels), `train.txt`, `val.txt`, etc. with a few minor modifications.

First when running the `convert_data.py` script, you will need to change the `--ci_path` and the `--out_path` from what they were before so as not to overwrite the previous data.  If you would like for your `.pbtxt` file to be in the same directory as the CIdevkit as before (so as not to overwrite the old one), you will need to modify one line of code.  In `${HOME}/ci-models/utils.py` you will need to change `data` in line 285 to `dac-data`.  After changing this, the `.pbtxt` file should be written in the correct directory.  An example is shown below for running the convert_data script with the new paths:

    # Convert the data to have the correct file structure
    # From ci-models directory
    python convert_data.py \
        --ci_path ${HOME}/ci-models/dac-data/20-class/ \
        --out_path ${HOME}/ci-models/dac-data/CIdevkit/

This will create the dataset (in the same way as before) inside the `dac-data/CIdevkit` directory except this time for 20 classes instead of 1 class.  For the other scripts (i.e. `create_trainval_set.py` and `create_ci_tfrecords.py`), you will need to modify the execution parameters to be consistent with where you put the data.

After your TFRecords (and other supporting files) have been created properly, you can continue with training using the same process as before.  Be careful to modify the `.config` file properly to let the network know you have a dataset with 20 classes now instead of 1.  The same training/evaluation process should be completed without much modification.  


## Tensorboard

During the training or evaluation process, you most likely specified a training directory (${TRAIN_DIR}) or and evaluation directory (${EVAL_DIR}).  If these processes were completed correctly, you should have some files beginning with `event.###`.  These files are generated specifically for use with [Tensorboard](https://www.tensorflow.org/programmers_guide/summaries_and_tensorboard) which you can use to visualize test images, training/evaluation loss and accuracies, and even view the "graph" or network layout that you used for training.  While on a GPU node (K20, K40, P100) and your Anaconda environment activated, simply issue the following command to activate the Tensorboard UI:

    tensorboard --logdir=${LOGDIR}  # Where ${LOGDIR} is the directory with training/eval `event` files

Once you do this, you will need to forward the Tensorboard port to your local machine so that you can view it in your browser.  To do this follow these steps:

1. Open a new terminal
2. SSH into Palmetto again, this time using a special syntax with port forwarding:
    ```
    ssh -L 16006:${NODE}.palmetto.clemson.edu:6006 ${USER}@user.palmetto.clemson.edu
    # Replace ${NODE} with the node name where you started Tensorboard (i.e. node1234)
    # Replace ${USER} with your Palmetto username
    ```
3. Open a local browser and navigate to the following URL:
    ```
    localhost:16006
    ```

This should show you Tensorboard now that is running on Palmetto but forwarded to port 16006 on your machine.


<a name="convolution" />

# Introduction to Convolution

The video linked [here](https://www.youtube.com/watch?v=bNb2fEVKeEo&list=PLC1qU-LWwrF64f4QKQT-Vg5Wr4qEE1Zxk&index=5) will be used for demonstration purposes for `convolution` and `convolutional layers`: 

To introduce convolutional layers, we must first talk about convolution.  Convolution is the act of taking a "filter" and sliding it across an image.  The effect of this can be many-fold: edge detection, image sharpening, image blurring, and many more.  To learn more about different convolutional filters in general, visit [Kernel Wiki](https://en.wikipedia.org/wiki/Kernel_(image_processing)).  Below you can see an animation of what a convolution is doing when slide across an image, computing the sum of each pixel it overlaps with as it goes to produce the output.

![](images/convolution.gif)

Up to this point, the theoretical information that we have provided has been in regards to fully connected networks.  If you remember, all neurons in a single layer are connected to all neurons in previous and subsequent layers.  However, for larger image sizes, this becomes very computationally expensive very quickly.  For this reason (as well as it's implications with computer vision), the convolutional neural network was created.  The following image depicts a general architecture for a convolutional neural network:

![](images/cnn_arch.jpg)

Notice that there are what looks like *convolution filters* at each layer pointing to a single pixel in the next layer.  Typically stacks of convolutional layers are placed in a network to work as `feature extractors`, followed by a few fully connected layers that can compute probability values and determine a class prediction.  The network which everyone has been using so far as part of the training process in this CI is called [Single Shot Multibox Detector (SSD)](https://arxiv.org/pdf/1512.02325.pdf).  Below is a depiction of the SSD model.

![](images/ssd_arch.png)

As you can see, this network is made up almost solely of convolutional layers that perform feature extraction at different aspect ratios an provide those to the output.  For now, you can think of the detection layers and non-maximal suppression layers as fully-connected layers which produce the desired output.



<a name="optimization" />

# Optimization Techniques

Now that we have talked about fully-connected networks and convolutional networks as well as gone through the processes of creating data, training a network, and evaluating a network, how do we make these networks better?  And what do we mean by *better*?

There are two (possibly more) ways that a network can be made to perform *better*:

* Make the network perform inference faster
* Make the network perform more accurately  

To make networks that perform inference faster (typically, this also causes accuracy to decrease) the size of the network must be decreased.  This can be done through pruning, removing entire layers, and a few other methods.  A good visualization of what a smaller network would look like is below.  As networks get larger, there are more matrix multiplications that must be computed; this in turn will cause a slower inference time.

![](images/cnn-comparison.png)

However, for now we will stick to the original architecture; instead we will be modifying training hyper-parameters that will allow the network to become more accurate.  A few of the available hyper-parameters that are available with the SSD-Inception networks that you have been training include:

* <a href="#bs">batch size<a/>
* <a href="#epochs">epochs/training steps</a>
* <a href="#lr">learning rate</a>
* <a href="#lrs">learning rate strategy</a>
* <a href="#momentum">momentum</a>
* <a href="#anchors">SSD anchor boxes</a>

<a name="bs" />

### Batch Size

If you check out the config file that you used for training, there is a parameter in the `train_config` section called `batch_size`.  **Batch size** defines the number of samples that is propagated through the network before a weight update is made (i.e. backpropagation).

For example, let's say there are 1000 trainings sample in your training set and you have a `batch_size` that is set to 50.  During the process of training, the first 50 samples from the training set are pushed through the network, errors for each sample are calculated, and the weights of the network are updated based on a combination of all the errors.  Next the second set of 50 samples are used with the same process until all samples in the training set have been exhausted.

> NOTE: Lower `batch_size` requires less memory so with a GPU with less memory, a lower `batch_size` is necessary.  However, smaller batch sizes will typically have a less accurate estimate of the gradient meaning that it will take longer to train a network.

The following image shows the difference in convergence on a solution with differing batch sizes (Red represents a batch size of 1, Green represents a slightly larger batch size, and Blue represents a large batch size):

![](images/batch_size.png)

<a name="epochs" />

### Epochs \& Training Steps

When determining how many long a network should be trained, two terms get thrown around: steps/iterations and epochs.  

**Steps/Iterations** are an pass through a single batch of data.  For example, if you have a `batch_size` of 25, 25 images will be passed through the network and weight updates will occur; this constitutes a **step** or **iteration**.

**Epochs** on the other hand, is a pass through the entire training set.  For example, if you have a 'batch_size` of 25 and a training dataset with 1000 images, 1 **epoch** will perform 40 **iterations** of the weight update process.  Depending on the framework you are using, configuration files could ask for iterations/steps or epochs for which you would like to train.

> NOTE: Be careful with the different in these as 1000 iterations is much different than 1000 epochs.

You may be inclined to think that just adding more steps will lead to better accuracy.  However, there is a sweet spot where your network is trained and can not learn anymore information.  The following image illustrates what happens with the process of **overtraining** or **overfitting**:

![](images/overfitting.png)

Notice that the accuracy on the training set either keeps increasing or remains constant; however the `testing` or `validation` set accuracy (after a point) starts to decrease.  This happens whenever you train your network for too long.  What begins to happen is the network starts to **memorize** the training set rather than learning useful information about the data that can be used to generalize to the `validation` set.  Finding that tradeoff point requires running `validation` at certain points throughout training to make sure you are not overfitting to the training data.

<a name="lr" />

### Learning Rate

**Learning rate** determines how drastic changes are made to the weights of a network to try and drive it to a particular solution.  Higher learning rates could potentially get your network to a solution quicker; however, there is always the change with higher learning rates that the network overshoots the optimal solution. Conversely, lower learning rates do not have as high a potential to overshoot a solution; however, they may have problems when local minima are involved.  If the learning rate is too low, it will not be able to escape a local minima and the accuracy will not be changed.  The figure below illustrates a learning rate that is too high on the left and too low on the right.

![](images/learning_rate1.png)

As we are trying to optimize the loss function when we are training a neural network, we can show the effects of different learning rates.  With learning rates that are too high, loss values tend to increase rather than decrease which learning rates that are too high lead to a non-optimal solution.  The figure below illustrates loss values based on different learning rates.

![](images/learning_rate2.jpeg)

Lastly, when determining learning rates, network developers tend to use a strategy where the learning rate decreases over the training process.  In effect, this will explore the error space with larger steps at the beginning of training, but once training has been going for a while, a lower learning rate is used to hone in on a solution.

<a name="lrs" />

### Learning Rate Strategy

**Learning rate strategy** can be defined as the shape of the learning rate curve (typically, as mentioned before, from higher learning rate to lower learning rate).  There are two typical learning rate strategies that are used for training: step and exponential.  Examples of each of these can be seen below.  Depending on your training task, exponential or step could perform better (depending on the error space for the optimization function).

<img src="images/step_lr.jpeg" width="325"/> <img src="images/exp_lr.jpeg" width="325"/>

<a name="momentum" />

### Momentum

**Momentum** simply adds a fraction of an update to the current one when you are performing the *weight update*.  For example, if your weight updates are always following a single path to the minimum the the momentum term of your *optimization* equation will help get to the solution a little faster.  You can imagine with a high learning rate and a momentum, you will most likely overshoot the optimal solution.  Therefore, lower learning rates are typically used with momentum values.  You can see that in the middle plot in the below figure it takes more steps without momentum than with momentum (in the right plot).

![](images/momentum.jpg)

<a name="anchors" />

### SSD BBox Anchors
The networks that you have been training up to this point have all been based on the SSD architecture (shown below again):

![](images/ssd_arch.png)

This architecture was built so as to be able to find many different objects with different aspect ratios in the image.  For example, you should be able to detect both small and large objects with the same network.  This is done through what are called `anchor boxes`.

![](images/ssd_grid.png)

The way the SSD network works (in a general sense) is as follows:

1. Images are separated into grids
2. A set of preset anchors is defined
3. Present anchors are used in each cell to determine probabilities of objects

Notice that in the above image, there are 5 different aspect ratio boxes that are generated for a given image cell.  These aspect ratios in each cell each have a probability associated with them (the chances of their being an object inside that box).  Based on the dataset, these aspect ratios can be changed to be able to locate smaller and larger objects.  A set of the initial aspect ratios can be found in the configuration files that were used for training.

<a name="qwiklab" />

# Qwiklab Jupyter Notebooks


<a name="final" />

# Final Project

For the final project for this CI, you will choose a dataset and then explore the different models that are available with the [Tensorflow Object Detection API](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md).  Make sure to note that there are many different models available for training (i.e. Faster RCNN, SSD-Mobilenet, SSD-Inception, RFCN, etc.).  A suggestion for the final project would be to choose a dataset and test a number of different network architectures (i.e. SSD-Mobilenet vs. SSD-Inception) and report the accuracy of each of the models.  By using some of the concepts discussed in the presentations this semester, you should be able to run a few experiments with differing hyperparamaters (i.e. learning rate, decay, batch size, etc.) to try and gain the most performance out of the model as possible.

Since we have been delving into object detection, you should choose a dataset which contains labels for an object detection task.  A few are give in the following list:

* COCO: [link](http://cocodataset.org/#home)
* KITTI: [link](http://www.cvlibs.net/datasets/kitti/eval_object.php)
* DOTA (Aerial Drone Footage): [link](http://captain.whu.edu.cn/DOTAweb/)
* VIVA (Traffic Sign Detection): [link](http://cvrr.ucsd.edu/vivachallenge/index.php/signs/sign-detection/)

Some other datasets can be found [here](https://en.wikipedia.org/wiki/List_of_datasets_for_machine_learning_research#Object_detection_and_recognition) and [here](http://deeplearning.net/datasets/)

The preceding datasets are well know detection datasets that can be used for this project.  Some include normal everyday objects while others include road signs and other autonomous vehicle related objects.

For the final assignment you will choose one of these dataset, and train and evaluate multiple networks to determine the best fit for the dataset.  This means that you can train both the SSD Inception and SSD Mobilenet models and see which one performs the best (both in inference time and inference accuracy).

> NOTE: Depending on the dataset you choose, you may need to convert the labels into VOC format (or KITTI format) to make sure that you are able to use the TFRecord creator for your data.  If you are unable to create your TFRecords, please feel free to come and ask any questions and we can point you in the right direction for converting your data to the right format.

<a name="wrapup" />

# Semester Wrapup


<a name="faq" />

# Frequently Asked Questions

**Q: I am trying to run the scripts above and a message keeps appearing saying "No module named tensorflow".**

**A:** One of two things has occurred.  Either, you have not installed Tensorflow correctly, or you are not executing the code from inside your python environment.  For the former, you will need to follow the instructions to install tensorflow-gpu in your Anaconda environment; for the latter, you will simply need to activate your Anaconda environment (i.e. `source activate ci-py35`).

**Q: When submitting a training or evaluation job using a PBS script, an error message about Bad UID appears?**

**A:** To submit any PBS script you will need to be on the login node.  If you have run a `qsub` command to get a node with a GPU, you will need to `exit` that node to be able to run any PBS script.

**Q: Can I use any Anaconda, CUDA, and cuDNN version?**

**A:** Depending on the version of Tensorflow and the API that you have, you may need to use a specific version.  For example, if you are using CUDA 8.0, you will need to use cuDNNv6 as well as Tensorflow 1.4.#.  If you are using CUDA 9.0, you will need to use cuDNNv7 as well as Tensorflow 1.5.#.  The Anaconda version shouldn't matter, but should match every time you run the code.  Therefore, you should stick with a single version of Anaconda, CUDA, and cuDNN (place them in your `~/.bashrc` file) so as not to run into any versioning problems.

**Q: My training or evaluation job exited with an error message saying it could not find "libcuda.so".  What is the problem?**

**A:** Most likely, you are not on a node that contains a GPU (i.e. the login node).  You should request a node with a GPU and then try your training or evaluation again.  Also, make sure to request 2 GPUs on a single node to make sure there are no other users with access to the hardware on that node.

**Q: I tried to 'freeze' my model and got an error involving `layout_optimizer` method not found.  What should I do?**

**A:** This has been updated in the newest version of the API; however, instead of updating your API, simply remove it from the file (`exporter.py`).  Simply open `research/object_detection/exporter.py` and edit lines 71 and 72 as follows:

```python
# This is what these lines look like before changing
rewrite_options = rewriter_config_pb2.RewriterConfig(
    layout_optimizer=rewriter_config_pb2.RewriterConfig.ON)

# Change this to the following (i.e. remove `layout_optimizer`)
rewrite_options = rewriter_config_pb2.RewriterConfig()
```

**Q: I tried to run training and received the following error: `TypeError: __init__() got an unexpected keyword argument 'dct_method'`.  What should I do?**

**A:** This is a mistake in the newest version of the API as they forgot to remove a command line parameter when calling a function.  Therefore in the file 'research/object_detection/data_decoders/tf_example_decoder.py` you will edit line 110 (removing the `dct_method=dct_method`) while keeping the closing parentheses and the comma as follows:

```python
# This is what these lines look like before changing
fields.InputDataFields.image:
    slim_example_decoder.Image(
        image_key='image/encoded',
        format_key='image/format',
        channels=3,
        dct_method=dct_method),

# Change this to the following (i.e. remove dct_method argument completely)
fields.InputDataFields.image:
    slim_example_decoder.Image(
        image_key='image/encoded',
        format_key='image/format',
        channels=3),
```
