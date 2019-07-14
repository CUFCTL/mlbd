## Object Detection

This is the main page for object detection projects.

- [Darknet on Palmetto](darknet.md)
- [Label Detection](label-detection.md)

### Overview

Deep Learning is currently a very popular paradigm of machine learning research used for applications like image processing, natural language processing, and vehicle control (just to name a few).  Deep Learning has become so successful in recent years due in part to the availability of modern high-performance computing resources such as GPUs, but also because of the sudden influx of massive amounts of data for training huge neural networks.  The goal of this Creative Inquiry is to expose students to Deep Learning, gain an understanding of how networks operate, understand how network creation and performance is directly affected by the training set as well best practices for designing training sets to meet specific goals.  Students should expect exposure to high-performance computing hardware, cutting-edge machine learning software, and sensors used in dataset creation.

### Data Drive

A large data drive is present on an ECEFTL lab machine with space allocated for Creative Inquiry use.  This data drive is for use with code, datasets, resulting images, and any other relevant information that needs to be stored in a permanent location.  Each CI member will be given access to this data partition and will be given a directory solely dedicated for their own storage.  This should be where all data should be placed that needs to be stored long term or shared with the CI leaders, etc.  __DO NOT__ go into other user's directories and remove or create any data.  Only manipulate the data that is in your directory.

The directories are created with a user `dlbd` for which you will use when issuing commands like `scp` or `sftp` to copy data to/from your local machine to Palmetto to this machine.  The following commands will allow you to copy files to/from your local machine to palmetto, to the lab machine, and to/from palmetto and the lab machine.  Make sure that if you are copying files to/from the lab machine that you are either on campus or logged intot eh VPN as it won't work otherwise.  (NOTE: use the -r flag to copy an entire directory from one location to another, instead of file by file)

```
# Local to Palmetto
scp <path-to-file> <cu-username>@user.palmetto.clemson.edu:<destination-directory>
# Palmetto to Local
scp <cu-username>@user.palemtto.clemson.edu:<path-to-file> <destination-directory>

# Local to FCTL Machine
scp <path-to-file> dlbd@eceftl10.ces.clemson.edu:/data/ci-dlbd/users/<cu-username>/<destination-directory>
# FCTL Machine to Local
scp dlbd@eceftl10.ces.clemson.edu:/data/ci-dlbd/users/<cu-username>/<path-to-file> <destination-directory>

# Palmetto to FCTL Machine
scp <cu-username>@user.palmetto.clemson.edu:<path-to-file> dlbd@eceftl10.ces.clemson.edu:/data/ci-dlbd/users/<cu-username>/<destination-directory>
# FCTL Machine to Palmetto
scp dlbd@eceftl10.ces.clemson.edu:/data/ci-dlbd/users/<cu-username>/<path-to-file> <cu-username>@user.palmetto.clemson.edu:<destination-directory>
```
