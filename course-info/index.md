# Course Schedule

Below you will find information about the projects and software used during each semester of the creative inquiry.

| Week | Topic                                                          | Deliverable                           |
|:-----|:---------------------------------------------------------------|:--------------------------------------|
| 1    | [Introduction](#intro)                                         | Request a Palmetto account            |
|      |                                                                | Create Anaconda environment           |
| 2    | [Working with Data](#data-science)                             | Jupyter notebook                      |
| 3    | [Supervised Learning](#supervised-learning)                    | ""                                    |
| 4    | [Unsupervised Learning](#unsupervised-learning)                | ""                                    |
| 5    | [Neural Networks: Dense Layers](#neural-networks-dense)        | ""                                    |
| 6    | [Neural Networks: Convolutional Layers](#neural-networks-conv) | ""                                    |
| 7    | [Semester Project](#semester-project)                          | Semester Project                      |
| ...  | ...                                                            | ...                                   |
| 15   | [Tech-Elective Presentations](#tech-elective)                  | [Semester Report](#semester-report)   |

<a name="intro"/>

## Week 1: Introduction

Artificial intelligence (AI) is a growing field of research in our lab and around the world. We will introduce you to all the hot buzzwords (AI, machine learning, deep learning, data science, etc.) and to some of the work that we have done in our lab. We will give you an overview of what to expect from this creative inquiry in the coming semester.

In order to do anything with machine learning you have to learn the tools, and not just how to use the Linux command-line, but how to use the Linux command-line in a __high-performance computing__ (HPC) environment. We will show you how to get started with Clemson's HPC cluster, Palmetto. We will also show you how to use __Jupyter notebooks__, which are sort of like interactive Python worksheets, on Palmetto, without even using the command-line! We will do most of our work from this point with Jupyter notebooks.

### Deliverables

Go through the instructions on the [Getting Started](../skills/getting-started.md) page to get the Jupyter notebooks set up on Palmetto. After following the instructions you should be able to run the "Introduction" notebook on Palmetto via JupyterHub.

<a name="data-science"/>

## Week 2: Working with Data

The first and most important step when designing a machine learning system: __understand your data__. We will show you how to load, manipulate, and visualize data in Python. No more Excel worksheets like what they make you do in general engineering, we're going to do it in code. We will use several popular Python packages: __numpy__ and __pandas__ for handling data, and __matplotlib__ and __seaborn__ for visualization.

To download the Jupyter notebook to Palmetto:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/data-science.ipynb
```

### Deliverables

Once you've spent some time with the data science notebook, you'll be ready to start working towards your semester project. You will develop your project gradually as we go through each notebook, and then through the rest of the semester.

[Download the Semester Project Ideas presentation](https://cufctl.github.io/creative-inquiry/assets/semester-project-ideas.pdf)

<a name="supervised-learning"/>

## Week 3: Supervised Learning

Now that we know how to look at data in Python, let's use machine learning to understand it. We will look at a class of machine learning called __supervised learning__. In supervised learning, the model learns a task from labeled examples. One example is predicting the price of a house based on the house's age, area, location, etc; this kind of task is called __regression__. Another example is predicting whether a tumor is cancerous based on size, location, age of the patient, etc; this kind of task is called __classification__. We will learn how to train machine learning models to perform regression and classification in Python. We will use a Python package called __scikit-learn__.

To download the Jupyter notebook to Palmetto:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/supervised-learning.ipynb
```

<a name="unsupervised-learning"/>

## Week 4: Unsupervised Learning

Another class of machine learning is __unsupervised learning__. In unsupervised learning, the model learns a task from unlabeled examples, usually to find some kind of structure in the data. The most common unsupervised task is __clustering__, such as finding communities of people in a social network or communities of genes in a gene interaction network. Another common task is __dimensionality reduction__, such as transforming a dataset into 2D or 3D so that it can be visualized. We will look at clustering and dimensionality reduction in Python using __scikit-learn__.

To download the Jupyter notebook to Palmetto:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/unsupervised-learning.ipynb
```

<a name="neural-networks-dense"/>

## Week 5: Neural Networks: Dense Layers

Among the sea of machine learning algorithms, __neural networks__ stand out for their ability to scale to large and complex datasets. A neural network is a sequence of __layers__, and each layer performs a transformation from input to output. The most basic kind of layer is the __dense layer__ or __fully-connected layer__, which performs a linear transformation followed by a non-linear activation function. We will look at neural networks which use dense layers for a variety of tasks, such as classification.

To download the Jupyter notebook to Palmetto:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/neural-networks-dense.ipynb
```

<a name="neural-networks-conv"/>

## Week 6: Neural Networks: Convolutional Layers

One of the biggest applications of machine learning right now is __computer vision__, in which machines learn to see like humans through image processing. Images tend to contain a lot of data; a 100x100 grayscale image, which is small by modern standards, is effectively a 10,000-dimensional vector! Any machine learning algorithm we've studied up to this point, including the basic neural network, would have trouble scaling up to data of this size. Fortunately, we can take advantage of the 2D structure of images using __convolution__, and we can super-charge a neural network with __convolutional layers__, which allow the network to learn patterns in the image data in a very efficient way. Neural networks that incorporate convolutional layers are called __convolutional neural networks__ (CNNs). We will look at CNNs for various computer vision tasks, such as image classification.

To download the Jupyter notebook to Palmetto:
```bash
wget https://cufctl.github.io/creative-inquiry/notebooks/neural-networks-conv.ipynb
```

<a name="semester-project"/>

## Semester Project

For the remaining weeks in the course, we will only meet for you to provide updates so that we can help you through any hurdles in your project. This will be a good time to share ideas and issues with other students as they might be able to help you.

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

## Advanced Topics

Here are some topics that we would like to present on at some point in the future. These topics are more advanced but are becoming increasingly important in the realm of machine learning.

### Recurrent Neural Networks

So far we've used machine learning to perform tasks on all sorts of data, but there's one type of data we haven't encountered: sequential data. Sequential data is any kind of data where the sequence, or order of the samples is important. One type of neural network which is designed for sequential data is called a __recurrent neural network (RNN)__, in which there are actually feedback loops between neurons. There are many examples of sequential data, including text, audio, and financial data, and many type of recurrent models for learning from sequential data, such as __long short-term memory (LSTM)__.

### Generative Models

Most of the machine learning models that we have used so far are __discriminative models__, that is, they take in data and output some kind of information about the data, such as a class or cluster assignment. There is another class of models called __generative models__, which learn how to generate data from random input. There are two types of generative models which are popular right now, variational autoencoders (VAEs) and generative adversarial networks (GANs).

### Reinforcement Learning

In the course we learn about supervised and unsupervised learning, but there is a third area within machine learning that is a world of it's own. __Reinforcement learning__ is about training an __agent__ to do __sequential decision making__ in an __environment__. Think AIs that play games (AlphaGo, AlphaZero) or drive cars (Tesla Autopilot).
