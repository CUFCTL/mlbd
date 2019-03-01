# Course Schedule

Below you will find information about the projects and software used during each semester of the creative inquiry.

| Week | Topic                                                          | Deliverable                           |
|:-----|:---------------------------------------------------------------|:--------------------------------------|
| 1    | [Introduction](#intro)                                         | Request a Palmetto account            |
| 2    | [Command-line Tools](#command-line)                            | Jupyter notebook                      |
| 3    | [Working with Data](#data)                                     | ""                                    |
| 4    | [Supervised Learning](#supervised-learning)                    | ""                                    |
| 5    | [Unsupervised Learning](#unsupervised-learning)                | ""                                    |
| 6    | [Neural Networks: Linear Layers](#neural-networks-linear)      | ""                                    |
| 7    | [Neural Networks: Convolutional Layers](#neural-networks-conv) | ""                                    |
| 8    | [Introduction to Semester Project](#semester-project)          | Develop a project idea                |
| 9    | Semester Project                                               | Semester Project                      |
| 10   | ""                                                             | ""                                    |
| 11   | ""                                                             | ""                                    |
| 12   | ""                                                             | ""                                    |
| 13   | ""                                                             | ""                                    |
| 14   | ""                                                             | ""                                    |
| 15   | [Tech-elective Presentations](#tech-elective)                  | [Semester Report](#semester-report)   |

<a name="intro"/>

## Week 1: Introduction

Artificial intelligence (AI) is a growing field of research in our lab and around the world. We will introduce you to all the hot buzzwords (AI, machine learning, deep learning, data science, etc.) and to some of the work that we have done in our lab. We will give you an overview of what to expect from this creative inquiry in the coming semester.

### Deliverable

Get access to Palmetto. Refer to the [Palmetto Cluster](../skills/palmetto-cluster.md) page for details.

<a name="command-line"/>

## Week 2: Command-line Tools

In order to do anything with machine learning you have to learn the tools, and not just how to use the Linux command-line, but how to use the Linux command-line in a __high-performance computing__ (HPC) environment. We will show you how to get started with Clemson's HPC cluster, Palmetto. We will also show you how to use __Jupyter notebooks__, which are sort of like interactive Python worksheets, on Palmetto, without even using the command-line! We will do most of our work from this point with Jupyter notebooks.

### Deliverable

Create a custom Anaconda environment and install all of the Python packages you will need for the course, create a Jupyter kernel for your environment, and run the "Introduction" Jupyter notebook on JupyterHub. Refer to the [Python](../skills/python.md) page for details.

To run the Jupyter notebook on JupyterHub you will need to download the notebook to Palmetto. The easiest way to do this is to login to Palmetto (either through the terminal or through JupyterHub) and use the `wget` command:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/introduction.ipynb
```

<a name="data"/>

## Week 3: Working with Data

The first and most important step when designing a machine learning system: __understand your data__. We will show you how to load, manipulate, and visualize data in Python. No more Excel worksheets like what they make you do in general engineering, we're going to do it in code. We will use several popular Python packages: __numpy__ and __pandas__ for handling data, and __matplotlib__ and __seaborn__ for visualization.

To download the Jupyter notebook to Palmetto:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/data-visualization.ipynb
```

<a name="supervised-learning"/>

## Week 4: Supervised Learning

Now that we know how to look at data in Python, let's use machine learning to understand it. We will look at a class of machine learning called __supervised learning__. In supervised learning, the model learns a task from labeled examples. One example is predicting the price of a house based on the house's age, area, location, etc; this kind of task is called __regression__. Another example is predicting whether a tumor is cancerous based on size, location, age of the patient, etc; this kind of task is called __classification__. We will learn how to train machine learning models to perform regression and classification in Python. We will use a Python package called __scikit-learn__.

To download the Jupyter notebook to Palmetto:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/supervised-learning.ipynb
```

<a name="unsupervised-learning"/>

## Week 5: Unsupervised Learning

Another class of machine learning is __unsupervised learning__. In unsupervised learning, the model learns a task from unlabeled examples, usually to find some kind of structure in the data. The most common unsupervised task is __clustering__, such as finding communities of people in a social network or communities of genes in a gene interaction network. Another common task is __dimensionality reduction__, such as transforming a dataset into 2D or 3D so that it can be visualized. We will look at clustering and dimensionality reduction in Python using __scikit-learn__.

To download the Jupyter notebook to Palmetto:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/unsupervised-learning.ipynb
```

<a name="neural-networks-linear"/>

## Week 6: Neural Networks: Linear Layers

Among the sea of machine learning algorithms, __neural networks__ stand out for their ability to scale to large and complex datasets. A neural network is a sequence of __layers__, and each layer performs a transformation from input to output. The most basic kind of layer is the __linear layer__ or __fully-connected layer__, which performs a linear transformation followed by a non-linear activation function. We will look at neural networks which use linear layers for a variety of tasks, such as classification.

To download the Jupyter notebook to Palmetto:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/neural-networks-linear.ipynb
```

<a name="neural-networks-conv"/>

## Week 7: Neural Networks: Convolutional Layers

One of the biggest applications of machine learning right now is __computer vision__, in which machines learn to see like humans through image processing. Images tend to contain a lot of data; a 100x100 grayscale image, which is small by modern standards, is effectively a 10,000-dimensional vector! Any machine learning algorithm we've studied up to this point, including the basic neural network, would have trouble scaling up to data of this size. Fortunately, we can take advantage of the 2D structure of images using __convolution__, and we can super-charge a neural network with __convolutional layers__, which allow the network to learn patterns in the image data in a very efficient way. Neural networks that incorporate convolutional layers are called __convolutional neural networks__ (CNNs). We will look at CNNs for various computer vision tasks, such as image classification.

To download the Jupyter notebook to Palmetto:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/neural-networks-conv.ipynb
```

<a name="semester-project"/>

## Week 8: Introduction to Semester Project

This week we will introduce you to the semester project, which you will work on for the remainder of the semester. We will showcase some of the projects that we are doing in the lab, and we'll give you a list of basic project ideas. You will develop your own project to work on, using these ideas and everything we've learned so far as inspiration. Your project should be something that you can work on for the remainder of the semester, depending on the number of credit hours you're taking.

For the remaining weeks in the course, we will only meet for you to provide updates so that we can help you through any hurdles in your project. Additionally, during the last couple of weeks while you are finishing up, we'll showcase a few more "exotic" machine learning methods and some of their applications, just for fun.

[Download the Semester Project Ideas presentation](https://cufctl.github.io/creative-inquiry/assets/semester-project-ideas.pdf)

<a name="semester-report"/>

## Semester Report

For the end of the semester you will write a report that summarizes your semester project and what you learned, as well as your feedback for the course; what you liked and didn't like, what you'd like to see more of, etc. We will provide a report template for you will all of the necessary sections. This report must be done in LaTeX; refer to the [LaTeX](../skills/latex.md) page for details.

[Download the LaTeX Report Template](https://cufctl.github.io/creative-inquiry/assets/report-template.tex)

<a name="tech-elective"/>

## Additional Requirements for Tech-Elective Students

If you are taking the CI as an ECE technical elective (or Honors credit), you must do two things __in addition to the semester report__:
- __Present your work__ to the rest of the group during the last meeting.
- Submit a __Jupyter notebook__ of your project such that anyone can reproduce your results.

Your presentation should essentially be derived from your report; that is, it should be a series of slides describing what you tried to do, what happened, what you learned, etc. Please use graphics in lieu of text where possible.

Your Jupyter notebook should walk through your entire project. It should contain any code that you ran as part of any experiments and results that you described in your report, as well as any important instructions. In other words, anyone should be able to reproduce all of your results simply by running your notebook. You can use the Jupyter notebooks provided on this website as a guide. You do not need to provide the data that you used, but you should provide instructions for how to obtain it.

## Showcases

Here are some topics that we would like to present on at some point in the future. These topics are more advanced but are becoming increasingly important in the realm of machine learning.

### Recurrent Neural Networks

So far we've used machine learning to perform tasks on all sorts of data, but there's one type of data we haven't encountered: sequential data. Sequential data is any kind of data where the sequence, or order of the samples is important. One type of neural network which is designed for sequential data is called a __recurrent neural network (RNN)__, in which there are actually feedback loops between neurons. We will present some examples of RNNs for different types of sequential data, including text, audio, and financial data.

### Generative Models

Most of the machine learning models that we have used so far are __discriminative models__, that is, they take in data and output some kind of information about the data, such as a class or cluster assignment. There is another class of models called __generative models__, which learn how to generate data from random input. We will present two types of generative models which are popular right now -- variational autoencoders (VAEs) and generative adversarial networks (GANs) -- and a couple of use cases for them.
